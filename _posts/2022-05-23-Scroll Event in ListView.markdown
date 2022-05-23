---
layout: post
title:  Scroll Events in VirtualFlow based Controls
date:   2022-05-23 11:37:35 +0530
categories: JavaFX
---

JavaFX has the following methods on Node to register scroll events either
via touch or mouse scroll wheel:

* [onScrollProperty](https://openjfx.io/javadoc/17/javafx.graphics/javafx/scene/Node.html#onScrollProperty)
* [onScrollStartedProperty](https://openjfx.io/javadoc/17/javafx.graphics/javafx/scene/Node.html#onScrollStartedProperty)
* [onScrollFinishedProperty](https://openjfx.io/javadoc/17/javafx.graphics/javafx/scene/Node.html#onScrollFinishedProperty)

These methods work on all Nodes except VirtualFlow based controls i.e.
ScrollPane, ListView, TableView, TreeView, TreeTableView, etc.

# Explanation

VirtualFlow used in these controls is responsible for scrolling the content.
The VirtualFlow has an event handler for scroll events which consumes the event
and doesn't allow it to bubble up to the control.
Therefore, event handler attached to the control for scroll events aren't invoked.

Q. Can't we just get rid of the event consumption in VirtualFlow
and let these controls behave like any other Node?

Unfortunately, no. Imagine a ListView placed inside a ScrollPane.
If VirtualFlow does not consume the event,
a scroll action will scroll both the ListView and the ScrollPane.

# Solution

In case a scroll event is required on one of these controls, an event filter can be used instead.
*N.B.* Do not consume these events on event capturing phase.

```
var listView = new ListView();
listView.addEventFilter(ScrollEvent.ANY, event -> {
    // code
});
```

# Reference

Comments on the following issues have some insights on why this design approach was taken:

* [JDK-8096155](https://bugs.openjdk.java.net/browse/JDK-8096155)
* [JDK-8096847](https://bugs.openjdk.java.net/browse/JDK-8096847)









