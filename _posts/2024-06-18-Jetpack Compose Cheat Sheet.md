---
layout: post
title: "Jetpack Compose sheet"
date: 2024-06-18
categories: [Android]
tags: [Android, Jetpack Compose]
---
## Jetpack Compose Cheat Sheet

Jetpack Compose is Android's modern toolkit for building native UI. It simplifies and accelerates UI development on Android with less code, powerful tools, and intuitive Kotlin APIs. This cheat sheet will provide an overview of the essential components and functionalities, along with brief explanations to help you get started.

### Basic Setup

### Gradle Dependencies

To get started with Jetpack Compose, you need to add the necessary dependencies to your app's `build.gradle` file:
```groovy
implementation "androidx.compose.ui:ui:1.0.0"
implementation "androidx.compose.material:material:1.0.0"
implementation "androidx.compose.ui:ui-tooling-preview:1.0.0"
implementation "androidx.lifecycle:lifecycle-runtime-ktx:2.3.1"
implementation "androidx.activity:activity-compose:1.3.0"
```

### Basic Composables

### Text

The `Text` composable is used to display text on the screen.
```kotlin
Text(text = "Hello, Jetpack Compose!")
```

### Button

The `Button` composable creates a button that can trigger actions when clicked.
```kotlin
Button(onClick = { /* Do something */ }) {
    Text("Click Me")
}
```

### Column

The `Column` composable arranges its children in a vertical sequence.
```kotlin
Column {
    Text("First item")
    Text("Second item")
}
```

### Row

The `Row` composable arranges its children in a horizontal sequence.
```kotlin
Row {
    Text("First item")
    Text("Second item")
}
```

### Box

The `Box` composable allows you to stack children on top of each other.
```kotlin
Box {
    Text("This is inside a Box")
}
```

### State Management

Managing state in Jetpack Compose is simple and efficient.

### Remember

`remember` helps to retain state across recompositions.
```kotlin
val counter = remember { mutableStateOf(0) }

Button(onClick = { counter.value++ }) {
    Text("Clicked ${counter.value} times")
}
```

### Layouts and Modifiers

Modifiers allow you to adjust the layout, appearance, and behavior of composables.

### Padding

Add padding around a composable.
```kotlin
Text(
    text = "Hello, Padding!",
    modifier = Modifier.padding(16.dp)
)
```

### Background Color

Set the background color of a composable.
```kotlin
Text(
    text = "Hello, Background!",
    modifier = Modifier.background(Color.Blue)
)
```

### Size

Set the size of a composable.
```kotlin
Box(
    modifier = Modifier
        .size(100.dp)
        .background(Color.Red)
)
```

### Material Design Components

Jetpack Compose provides a set of Material Design components to help you build beautiful UIs.

### Scaffold

`Scaffold` implements the basic material design layout structure.
```kotlin
Scaffold(
    topBar = { TopAppBar(title = { Text("My App") }) },
    content = { padding ->
        Text(
            text = "Hello, Scaffold!",
            modifier = Modifier.padding(padding)
        )
    }
)
```

### FloatingActionButton

A floating action button (FAB) is a circular button that triggers the primary action in your app.
```kotlin
FloatingActionButton(onClick = { /* Do something */ }) {
    Icon(Icons.Filled.Add, contentDescription = "Add")
}
```

### Snackbar

Snackbars provide brief feedback about an operation.
```kotlin
Snackbar(
    action = {
        Button(onClick = { /* Do something */ }) {
            Text("Retry")
        }
    }
) { Text("This is a snackbar") }
```

### Images

Displaying images in Jetpack Compose can be done from resources or URLs.

### Load Image from Resources

Use the `painterResource` function to load an image from the resources.
```kotlin
Image(
    painter = painterResource(id = R.drawable.my_image),
    contentDescription = "My Image"
)
```

### Load Image from URL (using Coil)

Coil is an image loading library that integrates well with Jetpack Compose.
```groovy
implementation "io.coil-kt:coil-compose:1.3.2"
```
```kotlin
Image(
    painter = rememberImagePainter("https://example.com/image.jpg"),
    contentDescription = "My Image"
)
```

### Lists

Jetpack Compose provides lazy components for efficiently displaying large sets of data.

### LazyColumn

`LazyColumn` is used to display a vertically scrolling list.
```kotlin
LazyColumn {
    items(100) { index ->
        Text("Item #$index")
    }
}
```

### LazyRow

`LazyRow` is used to display a horizontally scrolling list.
```kotlin
LazyRow {
    items(100) { index ->
        Text("Item #$index")
    }
}
```

### Navigation

Jetpack Compose has its own navigation component to handle navigation between composables.

### Navigation Component

Add the navigation-compose dependency to your `build.gradle` file.
```groovy
implementation "androidx.navigation:navigation-compose:2.4.0-alpha10"
```

Define a navigation graph using `NavHost` and `composable`.
```kotlin
@Composable
fun NavGraph(startDestination: String = "home") {
    val navController = rememberNavController()
    NavHost(navController, startDestination) {
        composable("home") { HomeScreen(navController) }
        composable("detail") { DetailScreen(navController) }
    }
}
```

### Animations

Jetpack Compose makes animations easy and intuitive.

### Simple Animation

Animate properties with `animate*AsState`.
```kotlin
val alpha = animateFloatAsState(
    targetValue = if (isVisible) 1f else 0f,
    animationSpec = tween(durationMillis = 1000)
)

Box(modifier = Modifier.alpha(alpha.value)) {
    Text("Fading Text")
}
```

### Preview

Use the `@Preview` annotation to see your composables in the Android Studio design view.

### Preview Composable

Annotate a composable function to preview it in the IDE.
```kotlin
@Preview(showBackground = true)
@Composable
fun DefaultPreview() {
    MyComposable()
}
```

### Resources

- [Jetpack Compose Official Documentation](https://developer.android.com/jetpack/compose)
- [Compose Pathway](https://developer.android.com/courses/pathways/compose)
- [Compose Samples](https://github.com/android/compose-samples)