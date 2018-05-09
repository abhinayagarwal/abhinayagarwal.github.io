---
layout: post
title:  FXML friendly custom controls
date:   2018-05-09 08:49:35 +0530
categories: JavaFX
---

JavaFX has support for extending containers and controls. Most of us writing these
extensions want them to be FXML friendly. JavaFX provides a number of annotations 
to help improve FXML support and you should definitely have 
them on your controls.

The annotations are as follows:

* @DefaultProperty
* @NamedArg
* @IDProperty

Lets have a look at each of these annotations and their usage in details:

# @DefaultProperty

Specifies a property to which child element(s) will be added or
set when an explicit child property tag is not defined in FXML.

For example:

`Pane` has a default property defined as "children", which corresponds to the children property.

```
@DefaultProperty("children")
public class Pane extends Region {
    ...
}
```

`ImageView` has default property defined as "image", which corresponds to the image property.

```
@DefaultProperty("image")
public class ImageView extends Node {
    ...
}
```

Consider the following FXML:

```
<Pane fx:id=”pane”>
    <children>
        <ImageView>
            <image>
                <Image url="">
            </image>
        </ImageView>
    </children>
</Pane>
```

Since `@DefaultProperty` is defined for both `Pane` and `ImageView`, we can re-write the FXML as:

```
<Pane fx:id=”pane”>
    <ImageView>
        <Image url="">
    </ImageView>
</Pane>
```

There are a few things to keep in mind while annotating a class with `@DefaultProperty`:

* Class should define a public default constructor
* Class should not have any constructor with parameters annotated with `@NamedArg`

# @NamedArg

Used to define arguments for a class which will be used by the `FXMLLoader`
to instantiate the class when its used inside a FXML.

For example, one of the constructor in `Image` is declared as:

```
public Image(@NamedArg("url") String url) {
    ...
}
```

This indicates that an Image can be declared in FXML with a url property.

```
<ImageView>
    <Image url="/icon.png"/>
</ImageView>
```

Just like `@DefaultProperty`, here are a few things you should keep in mind while 
annotating parameters with `@NamedArg`:

* `@NamedArg` should be used to define properties which can be injected as parameters
to the constructor.
* If the properties annotated with `@NamedArg` are not provided in the FXML, the FXMLLoader will:
    * Call the default constructor, if present.
    * Call the parameterized constructor with `null` values.
* `@DefaultProperty` and `@NamedArg` should not be used on the same class. In case they are used,
`@DefaultProperty` will not work.

# @IDProperty

Specifies a property which can be used by the `FXMLLoader` to set the value from `fx:id`. For all child classes of `Node`, it is mapped to the `id` property of `Node`.

In case a class extends from `Node` or any of its subclasses, you do not need to worry about this
property. Classes which do not extend from Node and desire FXML support, needs to declare
`@IDProperty`.

The following classes in JavaFX, which do not extend `Node`, use them:

* ContextMent
* MenuItem
* Tab
* TableColumnBase
* ToolTip

At the time of writing this post, `@IDProperty` is still a private API and is present
in `com.sun.javafx.beans` package. Therefore, all controls using this property will fail
to compile under JDK 9 and later. There is an 
[open issue](https://bugs.openjdk.java.net/browse/JDK-8090835) 
to move it to the public `javafx.beans` package.


As I stated in the beginning of the post, these annotations **should** be used 
on every custom control. If you are extending a Node which has these annotations
on them, you can choose to either skip or override them.

Lets make all controls FXML friendly!!!