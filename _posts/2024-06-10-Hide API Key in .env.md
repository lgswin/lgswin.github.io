---
layout: post
title: "The management of a sensitive information by .env"
date: 2024-06-10
categories: [ReactNative]
tags: [.env, api key]
---

## To securely manage sensitive information such as a public APK key in a React Native project, 
you can use environment variables and a .env file. Here's a step-by-step guide on how to set this up:


1. **Install `react-native-dotenv`**
    
    You can use the `react-native-dotenv` library to load environment variables from a `.env` file. First, install the library:
    
    ```bash
    npm install react-native-dotenv
    ```
    
2. **Create a `.env` File**
    
    Create a `.env` file in the root of your project and add your environment variables there:
    
    ```makefile
    PUBLIC_APK_KEY=your_public_apk_key_here
    ```
    
3. **Configure Babel to Use `react-native-dotenv`**
    
    Edit your `babel.config.js` file to include `react-native-dotenv`:
    
    ```jsx
    module.exports = {
      presets: ['module:metro-react-native-babel-preset'],
      plugins: [
        ['module:react-native-dotenv', {
          moduleName: '@env',
          path: '.env',
        }],
      ],
    };
    ```

    In my case : 
    ```jsx
    module.exports = function(api) {
      api.cache(true);
      return {
        presets: ['babel-preset-expo'],
        plugins: [
          "nativewind/babel",
          [
            'module:react-native-dotenv',
            {
              moduleName: '@env',
              path: '.env',
              blocklist: null,
              allowlist: null,
              safe: false,
              allowUndefined: true,
            },
          ],
        ],
      };
    };
    ```
    
4. **Use Environment Variables in Your Code**
    
    Now you can import and use the environment variables in your React Native components:
    
    ```jsx
    import { PUBLIC_APK_KEY } from '@env';
    
    const MyComponent = () => {
      console.log('Public APK Key:', PUBLIC_APK_KEY);
    
      return (
        <View>
          <Text>{PUBLIC_APK_KEY}</Text>
        </View>
      );
    };
    
    export default MyComponent;
    ```
    In my case : 
    ```jsx
      return (
        <ClerkProvider publishableKey={REACT_APP_CLERK_PUBLIC_KEY}>
    ```
    
1. **Add `.env` to `.gitignore`**
    
    To ensure that your `.env` file is not committed to your version control system, add it to your `.gitignore` file:
    
    ```bash
    # .env file
    .env
    ```
    