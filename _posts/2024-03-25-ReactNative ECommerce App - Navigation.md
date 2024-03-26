---
layout: post
title: "ReactNative app #3 : 
Create reactNative Firebase setup and getting database"
date: 2024-03-25
categories: [ReactNative, Firebase]
tags: [ReactNative, Firebase, database]
---
# Create reactNative Firebase setup and Add Post

Go to  https://console.firebase.google.com/

→ select web application

![11](https://github.com/lgswin/lgswin.github.io/assets/83533586/c53c1720-2291-44f2-b333-1652c1eb4c2d)

→ Register app (Comunity MarketPlace)

→ `npm install firebase`

→ add firebaseConifg.jsx in the root folder and copy firebaseconfig content into it.

→ add export before initializeApp `export const app = initializeApp(firebaseConfig);`

→ build > Make a database > Firestore database > Start a collection

→ add icons collections like furniture, car, properties, Hobby, Clothing, jobs

![22](https://github.com/lgswin/lgswin.github.io/assets/83533586/4f6f14bb-3d1b-4143-92f0-b2fbb7ecc4b1)


- [Add Firebase to your JavaScript project](https://firebase.google.com/docs/web/setup?_gl=1*zaeqc1*_up*MQ.._gaMjk5NjY0ODk2LjE3MTEzOTgyMzc._ga_CW55HF8NVTMTcxMTM5ODIzNy4xLjAuMTcxMTM5ODI1My4wLjAuMA..&gclid=Cj0KCQjwwYSwBhDcARIsAOyL0fi_yxf37idxgodIad6IlOP9j6XYKakF1tfVc-i0KtrxGLdH0z_tPv8aAkqhEALw_wcB&gclsrc=aw.ds)


```markdown
Apps
├── firebaseConfig.jsx
├── Apps
│   ├── HomeScreen.jsx
│   ├── ExploreScreen.jsx
│   ├── ProfileScreen.jsx
│   └── AddPostScreen.jsx
```

```jsx
import {View, Text} from 'react-native';
import React, { useEffect } from 'react'
import {app} from '../../firebaseConfig';
import {getFirestore,getDocs,collection} from 'firebase/firestore';

export default function AddPostScreen() {

    const db = getFirestore(app);
    
    useEffect(() => {
        getCategoryList();
    }, []) // only once

    const getCategoryList=async()=>{
        const querySnapshot = await getDocs(collection(db, 'Category'));

        querySnapshot.forEach((doc)=> {
            console.log("Docs:", doc.data());
        })
    }
}
```

if press AddPost. Terminal shows log

```bash
LOG  Docs: {"Car": "https://cdn-icons-png.flaticon.com/128/8431/8431017.png"}
LOG  Docs: {"Furniture": "https://cdn-icons-png.flaticon.com/128/8438/8438274.png"}
LOG  Docs: {"Jobs": "https://cdn-icons-png.flaticon.com/128/5079/5079335.png"}
LOG  Docs: {"Hobby": "https://cdn-icons-png.flaticon.com/128/2816/2816775.png"}
LOG  Docs: {"Properties": "https://cdn-icons-png.flaticon.com/128/4664/4664316.png"}
LOG  Docs: {"Clothing": "https://cdn-icons-png.flaticon.com/128/3704/3704721.png"}
LOG  Docs: {"icon": "https://cdn-icons-png.flaticon.com/128/3659/3659899.png", "name": "Electronics"}
```