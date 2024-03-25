---
layout: post
title: "ReactNative app #2 : 
Navigation"
date: 2024-03-25
categories: [ReactNative, Navigation]
tags: [ReactNative, Expo, Navigation]
---

This is to implement the navigation.

Go to `reactnavigation.org`

Install the required packages in your React Native project:

`npm install @react-navigation/native`

**Installing dependencies into an Expo managed project**

`npx expo install react-native-screens react-native-safe-area-context`

Go to navigators in the site.

Install the bottom-tabs dependency

`npm install @react-navigation/bottom-tabs`

add folders and jsx files to implement navigation with a basic structure by rnc

```
Apps
├── Navigations
│   └── TabNavigation.jsx
├── Screens
│   ├── HomeScreen.jsx
│   ├── ExploreScreen.jsx
│   ├── ProfileScreen.jsx
│   └── AddPostScreen.cs
```

- Import createBottomTabNavigatior() in TabNavigation.jsx
    
    → add tabs 
    

```jsx
import { Text, View } from 'react-native'
import React, { Component } from 'react'
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import HomeScreen from '../Apps/Screens/HomeScreen';
import ExploreScreen from '../Apps/Screens/ExploreScreen';
import AddPostScreen from '../Apps/Screens/AddPostScreen';
import ProfileScreen from '../Apps/Screens/ProfileScreen';

const Tab = createBottomTabNavigator();

export default class TabNavigation extends Component {
  render() {
    return (
      <Tab.Navigator>
        <Tab.Screen name='home' component={HomeScreen} />
        <Tab.Screen name='explore' component={ExploreScreen} />
        <Tab.Screen name='addpost' component={AddPostScreen} />
        <Tab.Screen name='profile' component={ProfileScreen} />
      </Tab.Navigator>
    )
  }
}
```

<img width="200" alt="n1" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/40e3439f-3ab9-4f45-ad8e-981291b8f565">

- decorate navigation tabs.
- icon : https://icons.expo.fyi/Index


```jsx
{% raw %}
export default class TabNavigation extends Component {
  render() {
    return (
      <Tab.Navigator screenOptions={{
        headerShown: false,
      }}>
        <Tab.Screen name='home' component={HomeScreen} 
            options={{
                tabBarLabel: ({color})=>(
                    <Text style={{color:color, fontSize:12, marginBottom: 3}}>Home</Text>
                ), 
                tabBarIcon: ({color, size}) => (
                    <Ionicons name="home" size={size} color={color} />
                )
            }}
        />
{% endraw %}
```

<img width="431" alt="n2" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/cc3f23c0-5d7d-44c2-9e6a-b008b45a9369">