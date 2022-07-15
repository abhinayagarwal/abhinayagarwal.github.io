---
layout: post
title:  Flags for JavaFX application
date:   2022-05-28 04:14:35 +0530
categories: JavaFX
---

JavaFX has various flags to either add debug logs or switch configuration.
However, there is no central location which lists all these flags.
There have been multiple attempts to officially do this but most of them failed:
* [Need an official document covering Prism flags](https://bugs.openjdk.java.net/browse/JDK-8091497)

This is my humble attempt to categorise and list all these flags:

### General

| Flag                   | Options                      | Details                                                     |
|------------------------|------------------------------|-------------------------------------------------------------|
| glass.platform         | Monocle                      |                                                             |
| jdk.gtk.verbose        | true                         | Verbose logging related to GTK on Linux                     |
| jdk.gtk.version        | 2,3                          | Toggle between GTK versions. Default is 3 since OpenJFX 11  |
| javafx.verbose         | true                         | Verbose logging for JavaFX                                  |
| monocle.platform       | Headless                     |                                                             |

### Accessibility

| Flag                   | Options                      | Details                                |
|------------------------|------------------------------|----------------------------------------|
| glass.accessible.force | true                         | Force enable a11y on older platforms   |

### Prism

| Flag                   | Options                      | Details                                                                                                                                                            |
|------------------------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| prism.debug            | true                         | Debug output in prism                                                                                                                                              |
| prism.debugfonts       | true                         | Debug output related to fonts                                                                                                                                      |
| prism.forceGPU         | true                         | Force prism to run in HW accelerated mode                                                                                                                          |
| prism.order            | j2d, d3d, es2, sw            | Graphics engines                                                                                                                                                   |
| prism.printallocs      | true                         | Print texture allocation data                                                                                                                                      |
| prism.printrendergraph | true                         | Print texture allocation data                                                                                                                                      |
| prism.trave            | true                         | Trace output in prism                                                                                                                                              |
| prism.text             | native,t2k,pisces,openpisces | Text engines                                                                                                                                                       |
| prism.showcull         |                              |                                                                                                                                                                    |
| prism.showdirty        | true                         | Draws overlay rectangles showing where the dirty regions were                                                                                                      |
| prism.showoverdraw     | true                         | Draws overlay rectangles showing not only the dirty regions, but how many times each area within that dirty region was drawn (covered by bounds of a drawn object) |
| prism.verbose          | true                         | Verbose output in prism  


### Scaling

| Flag                       | Options                        | Details   |
|----------------------------|--------------------------------|-----------|
| glass.win.uiScale          | percentage e.g. "125%"         | 
| glass.win.renderScale      | percentage e.g. "125%"         |
| glass.gtk.uiScale          | decimal e.g. "1.25"            |
| sun.java2d.uiScale         | percentage e.g. "125%"         |
| sun.java2d.uiScale.enabled | true                           |
| swt.autoScale              | number e.g. "125"              |


