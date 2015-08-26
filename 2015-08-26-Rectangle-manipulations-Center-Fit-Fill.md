---
title: Rectangle manipulations. Center. Fit. Fill.
subtitle: CGRect Extensions
author: Artem Krachulov
category: Extensions, Swift
tags: Center, Fit, Fill, Extension, Swift, CGRect, iOS
excerpt: "Center methods returns and sets origin coordinates, which place rectangle in center another rectangle. Fit and Fill methods returns scale value, to fit or fill the content size of rectangle to the another content size of rectangle, by maintaining the aspect ratio."
---

###  Center rectangle inside another rectangle

`func CGRectCenters(rect1: CGRect, inRect rect2: CGRect) -> CGRect` method returns rectangle with origin coordinates, which place `rect1` in center `rect2`.

```swift
public func CGRectCenters(rect1: CGRect, inRect rect2: CGRect) -> CGRect {

    return CGRect(origin: CGPointMake((CGRectGetWidth(rect2) - CGRectGetWidth(rect1)) / 2, (CGRectGetHeight(rect2) - CGRectGetHeight(rect1)) / 2), size: rect1.size)
}

// Example

var rect1 = CGRectMake(0, 0, 500, 300)
var rect2 = CGRectMake(0, 0, 100, 200)

CGRectCenters(rect2, inRect: rect1) // {x 200 y 50 w 100 h 200}

rect2 = CGRectMake(0, 0, 800, 500)

CGRectCenters(rect2, inRect: rect1) // {x -150 y -100 w 800 h 500}
```


`func centersRectIn(rect: CGRect)` method sets to `rect1` new origin coordinates, which correspond to center coordinates inside `rect2`.

```swift
extension CGRect {

    mutating func centersRectIn(rect: CGRect) {

        self = CGRectCenters(self, inRect: rect)
    }
}

// Example

var rect1 = CGRectMake(0, 0, 500, 300)
var rect2 = CGRectMake(0, 0, 100, 200)

rect2.centersRectIn(rect1) // {x 200 y 50 w 100 h 200}

rect2 = CGRectMake(0, 0, 800, 500)

rect2.centersRectIn(rect1) // {x -150 y -100 w 800 h 500}
```

###  Scale fit value, by maintaining rectangle the aspect ratio

`func CGRectFitScale(rect1: CGRect, toRect rect2: CGRect) -> CGFloat` method returns rectangle `rect1` scale value, to fit the content size of rectangle `rect1` to the another content size of rectangle `rect2`, by maintaining the aspect ratio.

```swift
public func CGRectFitScale(rect1: CGRect, toRect rect2: CGRect) -> CGFloat {

    return min(CGRectGetHeight(rect2) / CGRectGetHeight(rect1), CGRectGetWidth(rect2) / CGRectGetWidth(rect1))
}

// Example

var rect1 = CGRectMake(0, 0, 500, 300)
var rect2 = CGRectMake(0, 0, 100, 200)

CGRectFitScale(rect1, toRect: rect2) // 0.2
```

###  Scale fill value, by maintaining rectangle the aspect ratio

`func CGRectFillScale(rect1: CGRect, toRect rect2: CGRect) -> CGFloat` method returns rectangle `rect1` scale value, to fill the content size of rectangle `rect1` to the another content size of rectangle `rect2`, by maintaining the aspect ratio.

```swift
public func CGRectFillScale(rect1: CGRect, toRect rect2: CGRect) -> CGFloat {

    return max(CGRectGetHeight(rect2) / CGRectGetHeight(rect1), CGRectGetWidth(rect2) / CGRectGetWidth(rect1))
}

// Example

var rect1 = CGRectMake(0, 0, 100, 200)
var rect2 = CGRectMake(0, 0, 500, 300)

CGRectFillScale(rect1, toRect: rect2) // 5
```

##  Usage



Other useful extensions you can find on my [Extensions](https://github.com/artemkrachulov/SwiftExtensions) repository.
