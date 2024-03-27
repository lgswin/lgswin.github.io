---
layout: post
title: "ReactNative app #1 : 
Building an E-commerce App with React Native Expo and Clerk Authentication"
date: 2024-03-24
categories: [ReactNative, Authentication]
tags: [ReactNative, Expo, Clerk Authentication]
---

In this tutorial, I'll walk through setting up a basic e-commerce app using React Native Expo and integrating Clerk authentication for seamless login with Google.

*  Setting Up Expo and Basic Project Structure
First, let's create a new React Native Expo project:

```bash
npx create-expo-app my-app
```

Then, start the project:

```bash
npx expo start
```

<img width="250" alt="1" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/4ef3da58-3a29-41c5-b7b0-3c4c1931efc4">

* Installing Tailwind CSS and Configuring
Tailwind CSS offers a quick way to style your app. Install it along with NativeWind:

```bash
npm add nativewind
npm add --dev tailwindcss@3.3.2
```

Initialize Tailwind CSS:

```bash
npx tailwindcss init
```
→ Created Tailwind CSS config file: tailwind.config.js
```
comunity-marketplace
├── expo
├── assets
├── Apps
├── node_modules
├ App.js 
├ babel.config.js
└ tailwind.config.js
```

- tailwind.config.js
add any custom folder to apply tailwind css
```jsx
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./App.{js,jsx,ts,tsx}", "./<custom directory>/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

- babel.config.js

```jsx
module.exports = function(api) {
  api.cache(true);
  return {
    presets: ['babel-preset-expo'],
    plugins: ["nativewind/babel"],
  };
};

```

- install extensions
    - Tailwind CSS IntelliSense
    - **ES7+ React/Redux/React-Native snippets**

* Implementing UI Components
→ Start coding in App.js

```jsx
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { Text, View } from 'react-native';

export default function App() {
  return (
  <View className="flex-1 items-center justify-center bg-white">
      <Text className="text-[40px] text-red-400">Open up Ampp.js to start working on your app!</Text>
      <StatusBar style="auto" />
    </View>
  );
}asd
```

<img width="200" alt="2" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/318e1199-ba15-4d83-9730-4491984c420b">


* Make a login UI to integrate Clerk Authentication
- Login Screen UI
- 
```
├── assets
├── Apps
├    └── Screens
├           └── LoginScreen.jsx  
```

- LoginScreen.jsx (type rnf)

```jsx
import { View, Text } from 'react-native'
import React from 'react'

export default function LoginScreen() {
  return (
    <View>
      <Text>LoginScreen</Text>
    </View>
  )
}
```

- App.js

```jsx
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { Text, View } from 'react-native';
import LoginScreen from './Apps/Screens/LoginScreen';

export default function App() {
  return (
  <View className="flex-1 bg-white">
      <StatusBar style="auto" />
      <LoginScreen />
    </View>
  );
}
```

- Add login image

```
comunity-marketplace
├── assets
├    └── images
├           └── login.jpeg
```

```jsx
import { View, Text, Image, TouchableOpacity } from 'react-native'
import React from 'react'

export default function LoginScreen() {
  return (
    <View>
        <Image source={require('./../../assets/images/login.jpeg')} 
            className = "w-full h-[400px] object-cover"    
        />
        <View className="p-8 bg-amber-100 mt-[-10px] rounded-3xl shadow-md">
            <Text className="text-[30px] font-bold">Community Marketplace</Text>
            <Text className="text-[18px] text-slate-500 mt-6">Buy SEll Marketplace where you can sell old item and make real money</Text>
            <TouchableOpacity onPress={()=>console.log("SignIn")} className="p-4 bg-blue-500 rounded-full mt-20">
                <Text className="text-white text-center text-[18px]">Get Started</Text>
            </TouchableOpacity>
        </View>
    </View>
  )
}
```

<img width="200" alt="3" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/e61f9d5c-7f54-4704-aa62-617097bd1714">
![4](https://github.com/lgswin/lgswin.github.io/assets/83533586/eb54043b-6efd-4182-9ae6-55e354fc2bb0)

* Clerk authentication to use google authentication (clerk.com)
1) make acount on clerk.com
2) goto https://dashboard.clerk.com/
3) create application with email and google options

![5](https://github.com/lgswin/lgswin.github.io/assets/83533586/f799126d-df77-45f9-a983-a9072afbe80c)
![6](https://github.com/lgswin/lgswin.github.io/assets/83533586/28da9001-baea-4b9e-acf0-79c7a16814f0)

4) install clerk with expo

- https://clerk.com/docs/quickstarts/expo
- `npm install @clerk/clerk-expo`

5) Wrap <ClerkProvider> in app.js and copy and paste publishablekey

- Then, Add SignedIn, SignedOut in  App.js

```jsx
import { StatusBar } from 'expo-status-bar';
import React from 'react';
import { Text, View } from 'react-native';
import LoginScreen from './Apps/Screens/LoginScreen';
import { ClerkProvider, SignedIn, SignedOut } from '@clerk/clerk-expo';

export default function App() {
  return (
    <ClerkProvider publishableKey='pk_test_ZGFzaGluZy1idWZmYWxvLTAuY2xlcmsuYWNjb3VudHMuZGV2JA'>
    <View className="flex-1 bg-white">
      <StatusBar style="auto" />
      <SignedIn>
        <Text className="mt-10">You are signed in</Text>
      </SignedIn>
      <SignedOut>
        <LoginScreen />
      </SignedOut>
    </View>
    </ClerkProvider>
  );
}
```


→ OAuth sign-in

- make hooks/useWarmUpBrowser.tsx file

```jsx
import React from "react";
import * as WebBrowser from "expo-web-browser";
 
export const useWarmUpBrowser = () => {
  React.useEffect(() => {
    void WebBrowser.warmUpAsync();
    return () => {
      void WebBrowser.coolDownAsync();
    };
  }, []);
};
```

- `npx expo install expo-web-browser`
- add WebBrowser, OAuth, onPress event in LoginScreen.jsx

```jsx
import { View, Text, Image, TouchableOpacity } from 'react-native'
import * as WebBrowser from "expo-web-browser";
import React from 'react'
import { useWarmUpBrowser } from '../../hooks/useWamUpBrowser';
import { useOAuth } from "@clerk/clerk-expo";

WebBrowser.maybeCompleteAuthSession();

export default function LoginScreen() {
    useWarmUpBrowser();
    const { startOAuthFlow } = useOAuth({ strategy: "oauth_google" });
    const onPress = React.useCallback(async () => {
        try {
          const { createdSessionId, signIn, signUp, setActive } =
            await startOAuthFlow();
     
          if (createdSessionId) {
            setActive({ session: createdSessionId });
          } else {
            // Use signIn or signUp for next steps such as MFA
          }
        } catch (err) {
          console.error("OAuth error", err);
        }
      }, []);

  return (
    <View>
        <Image source={require('./../../assets/images/login.jpeg')} 
            className = "w-full h-[400px] object-cover"    
        />
        <View className="p-8 bg-amber-100 mt-[-10px] rounded-3xl shadow-md">
            <Text className="text-[30px] font-bold">Community Marketplace</Text>
            <Text className="text-[18px] text-slate-500 mt-6">Buy SEll Marketplace where you can sell old item and make real money</Text>
            <TouchableOpacity onPress={onPress} className="p-4 bg-blue-500 rounded-full mt-20">
                <Text className="text-white text-center text-[18px]">Get Started</Text>
            </TouchableOpacity>
        </View>
    </View>
  )
}
```

If press the Get Started and sign in the google account, then,

<img width="400" alt="7" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/7c0d5cd9-7549-452b-beea-b9eea27afe82">

