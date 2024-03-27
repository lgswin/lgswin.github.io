---
layout: post
title: "ReactNative app #4 : 
Formik and Picker to add post"
date: 2024-03-26
categories: [ReactNative, Formik and Picker]
tags: [ReactNative, Formik, Picker, Form, Add Post]
---
When it comes to creating forms in React Native applications, managing state and user input can be a complex task. Luckily, libraries like Formik provide a convenient way to handle form logic and validation. In this article, we'll explore how to integrate Formik into a React Native application to create a dynamic form with a category picker.

* Setting Up Formik

Go to `formik.org`

install Formik

`npm install formik --save`

- AddPostScreen.jsx → return ()

```jsx
import { Formik } from 'formik';
```

* Implementing the Form

```jsx
**return (
    <View className="p-10">
        <Formik
            initialValues={{title:'',desc:'',address:'',price:'',image:''}}
            onSubmit={value=>console.log(value)}
        >
            {({handleChange,handleBlur,handleSubmit,values})=>(
                <View>
                    <TextInput
                        style={styles.input}
                        placeholder='Title'
                        value={values?.title}
                        onChangeText={handleChange('title')}
                    />
                    <TextInput
                        style={styles.input}
                        placeholder='Description'
                        value={values?.desc}
                        numberOfLines={5}
                        onChangeText={handleChange('desc')}
                    />
                     <TextInput
                        style={styles.input}
                        placeholder='Price'
                        value={values?.price}
                        keyboardType='number-pad'
                        onChangeText={handleChange('price')}
                    />
                    {/* Category */}
                    <Button onPress={handleSubmit} 
                    className="mt-7"
                    title="submit" />
                </View>
                
            )}
        </Formik>
    </View>
	)
}**
```
<img width="300" alt="p1" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/41dabbbf-9ee7-4298-94ca-15ae14de1800">
* Adding a Category Picker
    
    https://docs.expo.dev/versions/latest/sdk/picker/
    
    install the dependency
    
    `npx expo install @react-native-picker/picker`
    

```jsx
import {Picker} from '@react-native-picker/picker';
```

```jsx
<Formik
    initialValues={{title:'',desc:'',address:'',price:'',image:''}}
    onSubmit={value=>console.log(value)}
>
    {({handleChange,handleBlur,handleSubmit,values,setFieldValue})=>(
    
      {/* Category */}
      <View style={{borderWidth:1, borderRadius:10,marginTop:15}}>
      <Picker
          selectedValue={values?.category}
          className="border-2"
          onValueChange={itemValue=>setFieldValue('category', itemValue)}
      >
          {categoryList.length>0&&categoryList?.map((item,index)=>(
              <Picker.Item key={index}
              label={item?.name} value={item?.name} />
          ))}
      </Picker>
      </View>
```

<img width="300" alt="p2" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/c751e195-a3fd-4972-8f07-d0339e2791dc">