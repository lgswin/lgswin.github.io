---
layout: post
title: "ReactNative app #8 : Stack Navigation"
date: 2024-04-02
categories: [ReactNative, Stack Navigation]
tags: [ReactNative, Stack.Navigator]
---

Stack navigation is a fundamental concept in building navigation systems for mobile applications. It enables the organization of screens in a hierarchical order, resembling a stack of cards, where each screen is placed on top of the previous one. In React Native, the @react-navigation/stack library provides an intuitive way to implement stack navigation.

## Installation and Setup:
Before diving into the code, let's ensure we have the necessary dependencies installed. Run the following commands in your terminal:

```
npm install @react-navigation/stack
npx expo install react-native-gesture-handler
```
These commands install the @react-navigation/stack library and react-native-gesture-handler, a dependency required for handling gestures in React Native applications.

## Creating a Stack Navigator:
Now, let's examine how to create a stack navigator using the createStackNavigator function provided by @react-navigation/stack. Below is an example code snippet:


## HomeScreenStackNav
```jsx
{% raw %}
const Stack = createStackNavigator();

export default function HomeScreenStackNav() {
  return (
   <Stack.Navigator>
    <Stack.Screen 
        name='home' 
        component={HomeScreen} 
        options={{
            headerShown: false,
        }}
    />
    <Stack.Screen 
        name='item-list' 
        component={ItemList} 
        options={({route}) => ({title: route.params.category,
            headerStyle: {
                backgroundColor:'#3b82f6'
            },
            headerTintColor:'#fff'
        })}
    />
        <Stack.Screen name='product-detail' 
        component={ProductDetail} 
        options={{
            headerStyle: {
                backgroundColor:'#3b82f6'
            },
            headerTintColor:'#fff',
            headerTitle:'Detail'
        }}
    />
   </Stack.Navigator> 
  )
}
{% endraw %}
```

Import createStackNavigator from @react-navigation/stack to create a new stack navigator.
Define a Stack constant to hold the stack navigator.
Within the HomeScreenStackNav function, we return the stack navigator component.
Inside the <Stack.Navigator> component, we define individual screens using the <Stack.Screen> component.
For each screen, we specify a name prop to uniquely identify it and a component prop to specify the React component to render.
Additionally, we can customize the navigation options for each screen using the options prop. In this example:
- For the 'home' screen, we set headerShown to false to hide the header.
- For the 'item-list' screen, we dynamically set the screen title based on the category parameter passed through navigation props (route.params.category). We also customize the header style and text color.


ItemList
```jsx
export default function ItemList() {
  const {params} = useRoute();
  const db=getFirestore(app); 
  const [itemList,setItemList]=useState([]);
  useEffect(()=>{
    params&&getItemListByCategory();
  },[params])

  const getItemListByCategory=async()=>{
    setItemList([]);
    const q=query(collection(db,'UserPost'), where('category','==', params.category));
    const snapshot=await getDocs(q);
    snapshot.forEach(doc=>{
        setItemList(itemList=>[...itemList,doc.data()]);
    })
  }
  return (
    <View className="p-2">
        {itemList?.length>0?
            <LatestItemList latestItemList={itemList} 
            heading={''}
            />
            :<Text className="p-5 text-[20px] mt-24 justify-center text-center text-gray-400">No Post Found</Text>
        }
    </View>
  )
}
```

## Categories
```jsx
      <FlatList
        data={categoryList}
        numColumns={4}
        renderItem={({item,index})=>(
          <TouchableOpacity 
          onPress={()=>navigation.navigate('item-list', {
            category:item.name
          })}
```

## LatestItemList
```jsx
export default function LatestItemList({latestItemList, heading}) {
  return (
    <View className="mt-3">
      <Text className="font-bold text-[20px]">{heading}</Text>
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

## ProductDetail
```jsx
export default function ProductDetail() {
    const {params}=useRoute();
    const [product,setProduct]=useState([]);

    useEffect(()=> {
        params&&setProduct(params.product);
    },[params])
    const sendEmailMessage=()=>{
        const subject='Regarding '+product.title;
        const body="Hi "+product.userName+"\n"+"I am insterested in this product";
        Linking.openURL('mailto:'+product.userEmail+"?subject="+subject+"&body="+body);
    }
  return (
    <ScrollView className="bg-white">
        <Image source={{uri:product.image}}
        className="h-[320px] w-full"
        />
        <View className='m-5'>
            <Text className="text-[24px] font-bold">{product?.title}</Text>
            <View className="items-baseline">
                <Text className="bg-blue-200 p-1 mt-2 px-2 rounded-full">
                    {product.category}
                </Text>
            </View>
            <Text className="mt-3 font-bold text-[20px]">Description</Text>
            <Text className="text-[17px] text-gray-500">{product.desc}</Text>
        </View>

        {/* User info */}
        <View className="p-3 flex flex-row items-center gap-3 bg-blue-100 border-gray-400">
            <Image source={{uri:product.userImage}}
                className="w-12 h-12 rounded-full"
            />
            <View>
                <Text className="font-bold text-[18px]">{product.userName}</Text>
                <Text className="text-gray-500">{product.userEmail}</Text>
            </View>
        </View>
        <TouchableOpacity 
            onPress={()=>sendEmailMessage()}
            className="z-40 bg-blue-500 rounded-full p-4 m-2">
            <Text className="text-center text-white">Send Message</Text>
        </TouchableOpacity>
    </ScrollView>
  )
}
```