---
layout: post
title: "How to Implement Multilingual Support in React Native"
date: 2024-06-18
categories: [ReactNative]
tags: [ReactNative, Multi language, localization]
---

Adding multilingual support to your React Native app is a crucial step to reach a global audience. In this post, we will discuss how to add multilingual support to a React Native app using `react-i18next` and `expo-localization`. The following example code demonstrates the implementation.

### Prerequisites

First, you need to install the required packages:

```bash
npm install i18next react-i18next expo-localization
```

### Directory Structure

You can structure your project directory as follows:

```bash
/project-root
  /src
    /locales
      en.json
      ko.json
    i18n.js
  App.js
```

### JSON Translation Files

In the `/src/locales` directory, you should have translation files for each language. For example, the `en.json` and `ko.json` files might look like this:

```json
// en.json
{
  "welcome": "Welcome",
  "login": "Login"
}
```

```json
// ko.json
{
  "welcome": "환영합니다",
  "login": "로그인"
}
```

### i18n Configuration File

In the `/src/i18n.js` file, configure i18next and react-i18next. Use the following code:

```jsx
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import * as Localization from 'expo-localization';
import en from './locales/en.json';
import ko from './locales/ko.json';

export const languageResources = {
  en: { translation: en },
  ko: { translation: ko },
};

i18n
  .use(initReactI18next)
  .init({
    compatibilityJSON: 'v3',
    lng: Localization.locale,
    fallbackLng: 'en',
    resources: languageResources,
    interpolation: {
      escapeValue: false, // React already safes from xss
    },
  });

export default i18n;
```

### Using i18n in App.js

Now, import the i18n configuration into your `App.js` file and use it:

```jsx
import React from 'react';
import { Text, View } from 'react-native';
import { useTranslation } from 'react-i18next';
import './src/i18n';

const App = () => {
  const { t } = useTranslation();

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>{t('welcome')}</Text>
      <Text>{t('login')}</Text>
    </View>
  );
};

export default App;
```

### Conclusion

Now, when you run the app, the language will be automatically set based on the user's device settings. `expo-localization` is used to detect the user's locale, and `react-i18next` is used to manage the translations. This makes it easy to implement multilingual support in your React Native app.