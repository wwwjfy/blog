---
date: "2016-10-23"
layout: post
slug: custom-keyboard-in-ios
title: Custom Keyboard in iOS
categories: ['Tips']
tags: ['iOS', 'Keyboard']
---

**UPDATES:** By code reviewing the original method, a better (but still flawed) way comes up.

It is to align the bottom only, and whenever rotation happens, set the height of the subview to be the same as the parent view. In this way, we can inherit the predefined height of the keyboard.

```swift
override func viewDidAppear(_ animated: Bool) {
    super.viewDidAppear(animated)
    heightConstraint = contentView.heightAnchor.constraint(equalToConstant: self.view.frame.height)
    heightConstraint.isActive = true
}

override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()
    if (view.frame.height != heightConstraint.constant) {
        heightConstraint.constant = view.frame.height
    }
}
```

No `viewWillTransition` and `updateViewConstraints` needed.
I still don't like it, but it looks better than the previous one.

---

I'm working on an iOS keyboard extension recently. In which I've met some strange "presumptions" required to present it properly.

### Height

First is the height. If the default height is used, it's fine. But I'd like a bar on top of it, like an accessory view, like the candidate words shown in the builtin keyboard.

[The official guide](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) kindly provides sample code to set the height constraint, which (as expected) doesn't work right away. "Obviously", you need to contain a subview with constraint to "activate" it. This is mysterious, or is it convention?

One thing needs to be kept in mind, is that, because of this "feature", if the height is not specified via a constraint, the height will be decided by the subviews' related constraints. For example, if the keyboard container view has `Accessory view - Collection View - Bottom bar`, and the collection view has no height specified, the height would be sum of accessory view and button bar, and the collection view would be collapsed.

### Rotation

In my few experiments, rotation cannot be handled properly, for `UICollectionView` to re-layout. 
The ideal behavior is when the rotation happens or will happen, a function is called to update constraint, re-layout `UICollectionView`, etc.

The function exists, only (not very much) the right time.

```swift
// Triggered when the rotation
override func viewWillTransition(to size: CGSize, with coordinator: UIViewControllerTransitionCoordinator) {
    super.viewWillTransition(to: size, with: coordinator)
    coordinator.animate(alongsideTransition: nil) { context in
    	   // call updateViewConstraints
        self.view.setNeedsUpdateConstraints()
    }
}

// Update constraints
override func updateViewConstraints() {
    super.updateViewConstraints()
    myCollectionView.collectionViewLayout.invalidateLayout()
}
```

It turns out that `updateViewConstraints` is called before the frames of views actually changed. This makes sense, because `updateViewConstraints` is to give orders to views, which are not yet executed. I tried to print things in `viewDidLayoutSubviews`, and the result is, it's called both before and after frames changed, multiple times. I also don't want to put `invalidateLayout` here, to be called more than once.

At last, I had to use the trick to call it only once in `viewDidLayoutSubviews`.

```swift
override func viewDidLayoutSubviews() {
    super.viewDidLayoutSubviews()

    if (myCollectionView.frame.height != savedCollectionViewHeight) {
        savedCollectionViewHeight = myCollectionView.frame.height
        myCollectionView.collectionViewLayout.invalidateLayout()
    }
}
```

I hope I'm doing wrong, and Apple does provide a reasonable way (not subclassing only for that, not monitoring some notifications or property changes).


