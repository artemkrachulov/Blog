---
title: Round up a CGFloat in Swift
subtitle: CGFloat Extension
author: Artem Krachulov
category: Extension, Swift
tags: Round, CGFloat, Swift, Ceil, iOS
excerpt: "Round up a CGFloat value with multiplier in the smaller side."
---

Round a value with multiplier in the smaller side.

```swift
public func ceil(x: CGFloat, multiplier: CGFloat) -> CGFloat {

    // I some cases % not work properly
    let y = x / multiplier
    let floorY = floor(y)

    if y - floorY > 0 {

        return x < 0 ? (multiplier * floorY + multiplier) : multiplier * floorY
    } else {

        return x
    }
}

// Examples

ceil(1, 0.3) // 0.9

ceil(-0.75, 0.5) // -0.5

ceil(1.22, 0.02) // 1.22

ceil(-1.3, 0.01) // 1.22
```

Other useful extensions you can find on my [Extensions](https://github.com/artemkrachulov/SwiftExtensions) repository.