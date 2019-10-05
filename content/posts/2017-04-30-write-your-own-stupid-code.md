---
date: "2017-04-30"
layout: post
slug: write-your-own-stupid-code
title: Write your own stupid code
categories: ['Thoughts']
tags: ['iOS', 'Swift', 'ProtocolBuffers']
---

**TLDR:** For a simple iOS serialization/deserialization task, I wrote less than 100 lines of code, to replace previously used Protocol Buffers. It reduced the app size by 3MB, and the load time from 3 seconds to 0.6 seconds. With some code of reading and writing binary data in Swift.

---

Those libraries are amazing. They can do a lot of things, work in various environments, come with complete documents, and full support. Yes, they're good, but sometimes, some stupid lines may be enough, to meet your simple needs.

I was working on an iOS keyboard, providing stickers to paste in conversations. The users are mainly in China, who naturally want a Chinese input method with it. I was starting without any fancy features, just a static list of all the candidates satisfying the input string. The source is like

```
菝 2.53953332627 ba
鲅 5.56816311706 ba
```

The columns are the character, the frequency, and the pinyin. I'd like a structure

```swift
[
  "b": [("ba", "菝"), ("ba", "鲅")]
]
```

The candidates are ordered by frequency, so I just need to match if the pinyin has the prefix of whatever the user inputs.

I didn't even think about it and picked protobuf for the data serialization. Because I didn't know how to read/write binary data in Swift, and the generated data file was much smaller than a binary plist (1MB vs 6MB), protobuf seemed like a good choice.

I read from the source, compiled to a data file, put it in the iOS bundle, and when the keyboard started, the first thing it did was to read from the data file. It worked well, though the loading time was not very good (3 seconds), but I could live with it.

My intention was to reduce the app size when I wanted to replace protobuf. The framework (I was using [ProtocolBuffers-Swift](https://github.com/alexeyxo/protobuf-swift)) was 11MB before and 3MB after compression. It's a little bit too big for such a simple task. It would be totally fine if being used as the communication protocol with servers.

While I think Swift's `unsafe` interfaces to manipulate binary are not easy to learn and friendly to use, (Look at all those combinations of unsafe pointers!) it turned out it was not so difficult to do it. Create a rule of how data should be stored and a few lines of code would do.

To write a `Dictionary` to a byte array and write to a file:

```swift
import Foundation

struct Item {
    var word: String
    var frequency: Double
    var pinyin: String
}

var dicts = [String:[Item]]()

var buffer = [UInt8]()
dicts.forEach { prefix, items in
    // add prefix
    buffer.append(contentsOf: Array(prefix.utf8))
    // add count
    var count = items.count
    buffer.append(contentsOf: withUnsafeBytes(of: &count) { return [UInt8]($0) })
    // add each tuple
    dicts[prefix]?.forEach { item in
        buffer.append(UInt8(item.pinyin.utf8.count))
        buffer.append(contentsOf: Array(item.pinyin.utf8))
        buffer.append(UInt8(item.word.utf8.count))
        buffer.append(contentsOf: Array(item.word.utf8))
    }
}
try Data(bytes: buffer).write(to: URL(string: "file:///tmp/data")!)
```

And to read:

```swift
let data = try Data(contentsOf: URL(string: "file:///tmp/data")!)
let intSize = MemoryLayout<Int>.size
var index = 0
var candidates = [String: [(String, String)]]()

// file structure:
// - 1 byte prefix ("a", "b", etc),
// - candidates number (8 bytes, Int)
//   - pinyin
//     - how many bytes to read
//     - utf8 string
//   - word
//     - how many bytes to read
//     - utf8 string
while index < data.count - 1 {
    guard let prefix = String(bytes: [data[index]], encoding: .utf8) else { return }
    index += 1
    candidates[prefix] = []

    let subCount = Array(data[index..<index+intSize]).withUnsafeBufferPointer { UnsafeRawPointer($0.baseAddress!).load(as: Int.self) }
    index += intSize

    candidates[prefix] = (0..<subCount).flatMap { _ -> (String, String)? in
        var count = Int(data[index])
        index += 1

        guard let pinyin = String(bytes: Array(data[index..<index+count]), encoding: .utf8) else { return nil }
        index += count

        count = Int(data[index])
        index += 1

        guard let word = String(bytes: Array(data[index..<index+count]), encoding: .utf8) else { return nil }
        index += count
        return (pinyin, word)
    }
}
```

It may worth mentioning that, in the initial version of loading data, `candidates` was appended in each loop. That took about **14 seconds** to read. That was before I realized `Dictionary` and `Array` in Swift were implemented as structure, and structures in Swift are value types, meaning every change (`candidates[prefix].append()`) would allocate a new memory area. After I used the above version, it took only 0.6 seconds to load (from 3 seconds), which was a surprising bonus to the reduced app size.

---

What I learned from this is, though libraries can save a lot of time, it's a good idea to put more thoughts on the purpose and the other solutions before adopting any.

And thank you, [raywenderlich.com](https://www.raywenderlich.com/148569/unsafe-swift) and [Cereal](https://github.com/Weebly/Cereal), for inspiration and some detail on how Swift works with binary.
