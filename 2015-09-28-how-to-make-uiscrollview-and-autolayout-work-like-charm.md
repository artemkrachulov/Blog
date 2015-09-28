---
title: How to make UIScrollView and Autolayout work like charm
subtitle:
author: Artem Krachulov
category: Development, Tutorial
tags: ios, auto layout, scrollview, tutorial, constraints
excerpt: "The aim is to place a lot of complex objects (with dynamic content, such as UILabel or UITextView) in the view of UIScrollView and make it work after changing the orientation of the device with Autolayout. What cant be difficult with UIScrollView and Autolayout? Lets see..."
---

The aim is to place a lot of complex objects (with dynamic content, such as UILabel or UITextView) in the view of UIScrollView and make it work after changing the orientation of the device with Autolayout.

What cant be difficult with UIScrollView and Autolayout?

On first look nothing... but after several attempts my trying failed miserably, and all examples of Google works with only one orientation or with static object size. And what to do if you have a lot of UITextView or UILabel with text (dynamic content)? Unfortunately, such examples, I have not found. It gave me a chance to solve the problem yourself.

## Lets go!

Open our wonderful program Xcode, press **"Create a new Xcode project"** and create **"Single View Application"**. Next, enter the name and other settings. Select device leave **"Universal"**, as our **Scroll View** should work on all devices.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/1-new-project.jpg" alt="New Project">
</p>

By one click on the file _Main.storyboard_, which is located in the **"Navigator Area"** -> tab **"Project navigator"** open its editing window.

> #####Tip
>
> Use keyboard hotkey for navigating code **⇧**(Shift) + **⌘**(Command) + **O**, which Apple calls “Open Quickly…”

The file is opened for editing.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/2-scrollview-storyboard.jpg" alt="Scroll View storyboard">
</p>

Now looking for an object **Scroll View** in the area of ​​utility **"Utilities Area"** -> panel **"Library panel"** -> tab **"Objects"**. Scroll a list of objects, or type in the search - **Scroll View**. Drag it to the view **View** of our controller **ViewController**. Stretch **Scroll View** to the size of the parent object and add constaints to all sides. To do this, in the left panel with structure of our file _Main.storyboard_ find object **Scroll View**, press **⌃**(Control), move the cursor to the parent object. In the drop-list check all necessary constaints: **Tralling Space to Container Margin**, **Leading Space to Container Margin**, **Vertical Spacing to Top Layout Guide**, **Bottom Spacing to Top Layout Guide**.

> #####Tip
>
> To select multiple constaints hold **⌘(**Command).

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/3-scrollview-constraints.jpg " alt="Scrollview constraints list">
</p>

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/4-scrollview-constraints.jpg " alt="Scrollview constraints">
</p>

To the content of the object **Scroll View** did not move under the status bar, you can change the value of the vertical constaint **Vertical Spacing to Top Layout Guide** on `0`.

Now create file _xib_, where there will be content for the object **Scroll View**. In the tab **"Project navigator"** by right-clicking open the context menu and select **"New file..."**. Select **"IOS"** -> **"User Interface"** -> **"View"**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/5-new-xib.jpg " alt="New xib">
</p>

Type name **"View"**:

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/6-xib-name.jpg " alt="Xib name">
</p>

This file will be the content and the container with all inner elements. Drag a new object **View** from tab **"Objects"** on the view of an existing object **View**. Stretch it to the size of the parent object and add multiple items to the container like **Text View**. Stretch their width and place eache after other.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/7-view-xib.jpg " alt="Xib views">
</p>

Checkbox **"Scrolling enabled"** must be turned off for all objects **Text View**, all the vertical constaints must well understand each other and there was no need to set a fixed height value for the object **Text View**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/8-text-scroll.jpg " alt="Scroll checkbox">
</p>

Adding constaints for all objects **Text View** on the sides of the container: **Tralling Space to Container Margin**, **Leading Space to Container Margin** and among themselves: **Vertical**. The first object to the top of the container: **Top Space to Container**, last to the bottom - **Bottom Space to Container**. The height of the container should match the height of all the objects with constaints. Adding constaints to the content and content: **Top Space to Container**, **Tralling Space to Container** and **Leading Space to Container**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/9-view-constraints.jpg " alt="View constraints">
</p>

Specify the owner of our **xib** file. On the left in the editor, select the item **"File's Owner"**, on right in the tab **"Identity inspector"** set class **ViewController**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/10-xib-owner.jpg " alt="Xib owner">
</p>

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/11-xib-owner.jpg" alt="Xib owner class">
</p>

Now time to add outlets for our objects with controller class **ViewController**. Turn two windowed mode. In the right window open the file _Main.storyboard_, in the left - **ViewController**. Holding **⌃**(Control) drag the object **Scroll View** to the area of class, call outlet **scrollView**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/12-scrollview-outlet.jpg" alt="Scrollview outlets">
</p>

Change the left window to the file _View.xib_. Holding **⌃**(Control) drag the content object **View** and conteiner obejct **View** to the area of same class, call outlets **scrollViewContent** and **scrollViewContentInner**.

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/13-scrollviewcontent-outlet.jpg" alt="Scrollview content outlets">
</p>

<p align="center">
    <img src="http://www.artemkrachulov.com/wp-content/uploads/2015/09/14-scrollviewcontentinner-outlet.jpg" alt="Scrollview container outlets">
</p>

Close two windowed mode and open the file **ViewController**. Starting to edit. Add the initialization method `init?(coder aDecoder: NSCoder)`. In this method, we load our view **scrollViewContent** parent object **View** from _View.xib_ file and add as subview to **Scroll View**.

```swift
required init?(coder aDecoder: NSCoder) {
  super.init(coder: aDecoder)

  // Load xib to the view
  scrollViewContent = (NSBundle.mainBundle().loadNibNamed("View", owner: self, options: nil).first) as! UIView
}

override func viewDidLoad() {
  super.viewDidLoad()

  // Do any additional setup after loading the view, typically from a nib.

  scrollView.addSubview(scrollViewContent)
}
```

Next let's take the methods that work with constraints our controller, determining when the dimensions will change: `viewWillLayoutSubviews()` and `viewDidLayoutSubviews()`. And will go to follow scheme:

1. Specify the size of the content same as **Scroll View** (we can specify only the width);
2. Update the dimensions of the container and all of its objects;
3. Get the new size of the container and set the parameter `contentSize` of **Scroll View**;
4. Update the height of the content to match the parameter `contentSize`;

```swift
override func viewWillLayoutSubviews() {
  super.viewWillLayoutSubviews()

  // 1. Set new size to content View
  scrollViewContent.frame.size = scrollView.frame.size

  // 2. Layout subviews to get container height
  scrollViewContent.layoutSubviews()

  // 3. Set Scroll view content size
  scrollView.contentSize = scrollViewContentInner.frame.size

  // 4. Update content View height
   scrollViewContent.frame.size.height = scrollViewContentInner.frame.size.height
}

override func viewDidLayoutSubviews() {
  super.viewDidLayoutSubviews()

  // Update content View height totally
  scrollViewContent.frame.size.height = scrollViewContentInner.frame.size.height
}
```

Launching the project. Rotate, scroll ... everything works fine. The project you can download from this link [project on the Github](https://github.com/artemkrachulov/ResponsiveScrollView).