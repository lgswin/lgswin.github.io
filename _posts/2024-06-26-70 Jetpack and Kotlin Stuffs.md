---
layout: post
title: "Review Jetpack compose codes - theme"
date: 2024-06-29
categories: [Android]
tags: [Android, Jetpack Compose, Kotlin, Theme]
---


To set Theme 

![Untitled](https://github.com/lgswin/lgswin.github.io/assets/83533586/a7b91204-d0c8-4e4e-a0dd-5095dea22e16)

```kotlin
@Composable
fun BasicCodelabTheme(
    darkTheme: Boolean = isSystemInDarkTheme(),
    dynamicColor: Boolean = true,
    content: @Composable () -> Unit
) {
    val colorScheme = when {
        dynamicColor && Build.VERSION.SDK_INT >= Build.VERSION_CODES.S -> {
            val context = LocalContext.current
            if (darkTheme) dynamicDarkColorScheme(context) 
            else dynamicLightColorScheme(context)
        }
        darkTheme -> DarkColorScheme
        else -> LightColorScheme
    }
    val view = LocalView.current
    if (!view.isInEditMode) {
        SideEffect {
            (view.context as Activity).window.statusBarColor = colorScheme.primary.toArgb()
            ViewCompat.getWindowInsetsController(view)?.isAppearanceLightStatusBars = darkTheme
        }
    }

    MaterialTheme(
        colorScheme = colorScheme,
        typography = Typography,
        content = content
    )
}
```

- **DarkColorScheme** and **LightColorScheme** define color schemes for dark and light themes respectively.
- The **BasicCodelabTheme** function sets the application's theme, taking `darkTheme` and `dynamicColor` parameters.
- **SideEffect** is used to set the status bar color of the activity when the view is not in edit mode, and adjust the system status bar theme.
- **MaterialTheme** wraps actual UI content, applying the configured color scheme and typography.

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            BasicCodelabTheme {
                MyApp(modifier = Modifier.fillMaxSize())
            }
        }
    }
}
```

To wrap the `BasicCodelabTheme` and specify a default theme