---
layout: post
title:  Exploring Platform Preferences in JavaFX
date:   2024-04-06 12:57:00 +0530
categories: JavaFX
---

JavaFX 22 introduces several new features and enhancements.
One of the notable additions is the Platform preferences API, 
which provides developers with a convenient way to access platform-specific preferences and adapt their applications accordingly.
In this blog post, we'll explore the new APIs and demonstrate how to leverage them in your JavaFX applications.

# Platform Preferences API

The Platform Preferences API in JavaFX 22 exposes platform-specific preferences as a read-only ObservableMap of key-value pairs.
These preferences encompass a wide range of UI settings, including color schemes, background and foreground colors, and accent colors.
The API is designed to be platform-independent, providing a consistent interface for accessing preferences across different operating systems.

## ColorScheme
One of the key components of the Platform Preferences API is the ColorScheme enumeration, which defines two color schemes for the user interface: LIGHT and DARK.
These schemes specify whether applications should prefer light text on dark backgrounds or dark text on light backgrounds, respectively.
By incorporating the ColorScheme enumeration into their applications, developers can create themes that seamlessly adapt to the user's preferred color scheme.

## Platform.Preferences

The `Platform.Preferences` interface extends the `ObservableMap` interface to expose platform preferences as key-value pairs.
The interface defines properties for commonly used preferences such as color scheme, background color, foreground color, and accent color.

## Platform#getPreferences

The `getPreferences()` method is a static method provided by the `Platform` class.
It returns an instance of the `Preferences` class, which allows developers to access platform preferences.

# ColorScheme Usage

In this blog we will only look at the `ColorScheme` usage.
Let's consider a scenario where you want to adjust the theme of your JavaFX application based on the color scheme.

```
Preferences preferences = Platform.getPreferences();
ColorScheme colorScheme = preferences.getColorScheme();
if (colorScheme == ColorScheme.DARK) {
    // apply dark theme
} else {
    // apply light theme
}
```

We also would like to listen to any changes so this could move into a listener:

```
Preferences preferences = Platform.getPreferences();
preferences.colorSchemeProperty().addListener((observable, oldValue, newValue) -> {
    if (newValue == ColorScheme.DARK) {
        // apply dark theme
    } else {
        // apply light theme
    }
});
```

# Example

To demonstrate the usage, let's write a JavaFX application and update its styling based on the `ColorScheme`:

```
import javafx.application.Application;
import javafx.application.ColorScheme;
import javafx.application.Platform;
import javafx.application.Platform.Preferences;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.layout.StackPane;
import javafx.stage.Stage;

public class PlatformPreferencesDemo extends Application {

    @Override
    public void start(Stage primaryStage) {

        Label label = new Label("Hello, JavaFX!");
        StackPane root = new StackPane();
        root.getChildren().add(label);
        Scene scene = new Scene(root, 300, 200);

        // Apply platform color scheme preferences
        Preferences preferences = Platform.getPreferences();
        if (preferences != null) {
            ColorScheme colorScheme = preferences.getColorScheme();
            updateTheme(colorScheme, scene);
        }

        // Add a listener to update on color scheme changes
        preferences.colorSchemeProperty().addListener((observable, oldValue, newValue) -> updateTheme(newValue, scene));

        primaryStage.setScene(scene);
        primaryStage.setTitle("Platform Preferences Demo");
        primaryStage.show();
    }

    private void updateTheme(ColorScheme colorScheme, Scene scene) {
        if (colorScheme == ColorScheme.DARK) {
            // Remove light stylesheet, add dark stylesheet
            scene.getStylesheets().remove(getClass().getResource("light-theme.css").toExternalForm());
            scene.getStylesheets().add(getClass().getResource("dark-theme.css").toExternalForm());
        } else {
            // Remove dark stylesheet, add light stylesheet
            scene.getStylesheets().remove(getClass().getResource("dark-theme.css").toExternalForm());
            scene.getStylesheets().add(getClass().getResource("light-theme.css").toExternalForm());
        }
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

```dark-theme.css
/* Define styles for the dark color scheme */

/* Set background color */
.root {
    -fx-background-color: #333333;
}

/* Set text color */
.label {
    -fx-text-fill: white;
}
```

```light-theme.css
/* Define styles for the light color scheme */

/* Set background color */
.root {
    -fx-background-color: #f0f0f0;
}

/* Set text color */
.label {
    -fx-text-fill: black;
}
```

Here is a video of how the application reacts the appearance changes on a mac:

<video src="https://github.com/abhinayagarwal/abhinayagarwal.github.io/assets/3197675/3e450f96-b287-4267-b01a-b86ff3331aef" controls="controls" style="max-width: 730px;">
</video>

# Conclusion

The introduction of the Platform Preferences API opens up new possibilities for creating platform-aware applications that seamlessly integrate with the user's preferred settings.
By leveraging this API, developers can enhance the user experience and ensure consistency across different operating systems.
Whether it's adapting color schemes, customizing UI elements, or fine-tuning application behavior,
the Platform Preferences API provides a powerful toolkit for building modern JavaFX applications.

References:

* [CSR for Platform preferences API](https://bugs.openjdk.org/browse/JDK-8319138)

