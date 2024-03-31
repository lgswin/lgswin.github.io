---
layout: post
title: "ReactNative app #7 : Home (header, category, product list)"
date: 2024-03-26
categories: [ReactNative, components]
tags: [ReactNative, search, category, FlatList, Image]
---

## Configuration for header

Add folders and files.

```
Apps
├── Compoents
│   ├── Slider.jsx
│   ├── PostItem.jsx
│   ├── Categories.jsx
│   ├── Header.jsx
│   └── LatestItemList.cs
├── Screens
│   ├── HomeScreen.jsx
│   ├── ExploreScreen.jsx
│   ├── ProfileScreen.jsx
│   └── AddPostScreen.cs
```

Add dependency folder and file in a tailwind.config.js (to recognize tailwind css in those files)

```jsx
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./App.{js,jsx,ts,tsx}", "./Apps/**/*.{js,jsx,ts,tsx}", 
					  "./Components/Homescreen/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Add a basic code by rnf (reactNativeFunctionalCompoents)

```jsx
import { View, Text } from 'react-native'
import React from 'react'

export default function Header() {
  return (
    <View>
      <Text>Header</Text>
    </View>
  )
}
```

## User information

```jsx
import { useUser } from '@clerk/clerk-expo'

...

export default function Header() {
    const {user} = useUser();
```

Show the information on the information section

```jsx
return (
    <View>
        {/* User infor sectiion */}
        <View className="flex flex-row items-center gap-4">
            <Image source={{uri:user?.imageUrl}} 
                className="rounded-full w-12 h-12"
            />
            <View>
                <Text className="text-[16px]">Welcome</Text>
                <Text className="text-[20px] font-bold">{user?.fullName}</Text>
            </View>
        </View>
```

## Search input box

```jsx
	      {/* Search bar section  */}
	      <View className="p-2 px-5 mt-5 flex flex-row bg-white rounded-full border-[2px] border-blue-300">
	          <EvilIcons name="search" size={24} color="gray" />
	          <TextInput placeholder='Search' 
	              className="ml-2 text-[18px]"
	              onChangeText={(value) => console.log(value)}    
	          />
	      </View>
```

Include Header component in Homescreen.jsx

```jsx
  <View className="py-8 px-6 bg-white flex-1">
    <Header />
  </View>
```

<img width="442" alt="h1" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/1a5f1e17-9fca-4184-aa31-ac5652f92f07">

## Slider

- make new collection for images in firestore database

![h01](https://github.com/lgswin/lgswin.github.io/assets/83533586/3ed47da2-f72f-4cfb-a568-7409d3656527)

`{image:”url”}`

Add code in slider component

```jsx
import { View, Text, FlatList, Image } from 'react-native'
import React from 'react'

export default function Slider({sliderList}) {
  return (
    <View className="mt-5">
      <FlatList
        data={sliderList}
        horizontal={true}
        showsHorizontalScrollIndicator={false}
        renderItem={({item,index})=>(
          <View>
            <Image source={{uri:item?.image}}
              className="h-[200px] w-[330px] mr-3 rounded-lg object-contain"
              />
          </View>
        )}
        />
    </View>
  )
}
```

- Attach Slider at Homescreen

```jsx
import Slider from '../../Components/Homescreen/Slider'
import {collection, getDocs, getFirestore} from 'firebase/firestore';
import {app} from '../../firebaseConfig';
```

```jsx
  const db = getFirestore(app); // get firestore database
```

```jsx
// loading slider data
const [sliderList,setSliderList]=useState([]);
useEffect(()=>{
	getSliders();
},[])

const getSliders= async ()=>{
	setSliderList([]);
	const querySnapshot = await getDocs(collection(db, "Sliders"));
	querySnapshot.forEach((doc) => {
	console.log(doc.id, " => ", doc.data());
	setSliderList(sliderList=>[...sliderList,doc.data()]);
	});
}
```

```jsx
  return (
      <View className="py-8 px-6 bg-white flex-1">
        <Header />
        <Slider sliderList={sliderList}/>
      </View>
  )
```
<img width="447" alt="h2" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/93ad1fc5-9890-45c4-aff1-0b89377f1f7f">

## categoryList

homescreen

```jsx
import Categories from '../../Components/Homescreen/Categories';
```

```jsx
const [sliderList,setSliderList]=useState([]);
const [categoryList,setCategoryList]=useState([]);
useEffect(()=>{
  getSliders();
  getCategoryList();
},[])

const getCategoryList=async()=>{
  setCategoryList([]);
  const querySnapshot = await getDocs(collection(db, 'Category'));

  querySnapshot.forEach((doc)=> {
      console.log("Docs:", doc.data());
      setCategoryList(categoryList=>[...categoryList,doc.data()]);
  })
  
return (
  <View className="py-8 px-6 bg-white flex-1">
    <Header />
    <Slider sliderList={sliderList}/>
    <Categories categoryList={categoryList}/>
  </View>
)
```

categories.jsx

```jsx
import { View, Text, FlatList, Image, TouchableOpacity } from 'react-native'
import React from 'react'

export default function Categories({categoryList}) { // {value} should be object
  
  return (
    <View className="mt-3">
      <Text className="font-bold textz-[20px]">Categories</Text>
      <FlatList
        data={categoryList}
        numColumns={4}
        renderItem={({item,index})=>(
          <TouchableOpacity className="flex-1 items-center justify-center p-2 border-[1px] border-gray-300 m-1 h-[80px] rounded-lg">
            <Image source={{uri:item?.icon}} // source={{uri:image}}
             className="w-[40px] h-[40px]"
            />
            <Text className="text-[12px] mt-1">{item.name}</Text>
          </TouchableOpacity>
        )}
        />
    </View>
  )
}
```

<img width="454" alt="h3" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/58d4e8b2-1b9f-4337-acca-4f7b9af8603c">

## Product list

- add created time

→ install moment (moment.js.com) to format date

`npm install moment  - - save`

→ add “createdAt” field in AddPostScreen.jsx

```jsx
initialValues={{... ,userImage:"",createdAt:Date.now()}}
```

- get user post

→ getLatestsItemList

```jsx
const getLatestItemList=async()=>{
  setLatestItemList([]);
  const querySnapshot=await getDocs(collection(db, 'UserPost'), orderBy('createdAt', 'desc')); 
  querySnapshot.forEach((doc) => {
    console.log("Docs:", doc.data());
    setLatestItemList(latestItemList=>[...latestItemList,doc.data()]);
  })
}
```

→ call  getLatestItemList

```jsx
const [latestItemList,setLatestItemList]=useState([]);
useEffect(()=>{
  getSliders();
  getCategoryList();
  getLatestItemList();
},[])
```

→ call the component

```jsx
  return (
      <ScrollView className="py-8 px-6 bg-white flex-1">
        <Header />
        <Slider sliderList={sliderList}/>
        <Categories categoryList={categoryList}/>
        <LatestItemList latestItemList={latestItemList}/>
      </ScrollView>
  )
```

## LatestItemList component

```jsx
import { View, Text, FlatList, Image, TouchableOpacity } from 'react-native'
import React from 'react'
import PostItem from './PostItem'

export default function LatestItemList({latestItemList}) {
  return (
    <View className="mt-3">
      <Text className="font-bold text-[20px]">Latest Items</Text>
      <FlatList
        data={latestItemList}
        numColumns={2}
        renderItem={({item,index})=>(
          <PostItem item={item} />
        )}
        />
    </View>
  )
}
```

## PostItem (reusable component)

```jsx
import { View, Text, TouchableOpacity, Image} from 'react-native'
import React from 'react'

export default function PostItem({item}) {
  return (
    <TouchableOpacity className="flex-1 m-2 p-2 rounded-lg border-[1px] border-slate-200">
        <Image source={{uri:item.image}} 
        className="w-full h-[140px] rounded-lg "
        />
        <View>
            <Text className="text-[15px] font-bold mt-2">{item.title}</Text>
            <Text className="text-[20px] font-bold text-blue-500">$ {item.price}</Text>
            <Text className="text-blue-500 bg-blue-200 mt-1 p-1 text-center rounded-full px-1 text-[10px] w-[70px]">{item.category}</Text>
        </View>
    </TouchableOpacity>
  )
}
```

<img width="413" alt="h4" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/b9ac9ecb-3476-4ffe-92d2-b08e4b6c9180">