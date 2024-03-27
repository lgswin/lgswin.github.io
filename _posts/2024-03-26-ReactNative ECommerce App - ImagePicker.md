---
layout: post
title: "ReactNative app #5 : ImagePicker"
date: 2024-03-26
categories: [ReactNative, ImagePicker]
tags: [ReactNative, ImagePicker]
---

# Create reactNative React Image Picker

Load a local image

```jsx
<Image source={require('./../../assets/images/placeholder.png')} 
    style={{width:100,height:100,borderRadius:15}}
/>
```

get expo image

https://docs.expo.dev/versions/latest/sdk/imagepicker/

`npx expo install expo-image-picker`

Add plugin in app.json

```json
    "web": {
      "favicon": "./assets/favicon.png"
    },
    "plugins": [
      [
        "expo-image-picker",
        {
          "photosPermission": "The app accesses your photos to let you share them with your friends."
        }
      ]
    ]
  }
}
```

Implement ImagePicker

```jsx
import * as ImagePicker from 'expo-image-picker';
```

```jsx
export default function AddPostScreen() {
    const [image, setImage] = useState(null);
    ...
		**const pickImage = async () => {
		        // No permissions request is necessary for launching the image library
		        let result = await ImagePicker.launchImageLibraryAsync({
		          mediaTypes: ImagePicker.MediaTypeOptions.All,
		          allowsEditing: true,
		          aspect: [4, 3],
		          quality: 1,
		        });
		    
		        console.log(result);
		    
		        if (!result.canceled) {
		          setImage(result.assets[0].uri);
		        }
		      };
		    
		    return (
		    ...**
```
<img width="435" alt="111" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/aa870ad9-95f9-4c7b-ae17-5d2cfa0ed154">


Then download some images on your device or simulator. then replace a image that set by the picker.

```jsx
{({handleChange,handleBlur,handleSubmit,values,setFieldValue})=>(
    <View>
        <TouchableOpacity onPress={pickImage}>
					{image?
					<Image source={{uri:image}} style={{width:100,height:100,borderRadius:15}} />
					:
					<Image source={require('./../../assets/images/placeholder.png')} 
					style={{width:100,height:100,borderRadius:15}}
					/>
					}
```

<img width="428" alt="222" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/0c37c3ce-9eb2-41ad-a3f8-5384fa4002e6">

update value for uri and validation.

- add new method

```jsx
    const onSubmitMethod=(value)=>{
        value.image=image;
        console.log(value);
    }

    return (
```

- change method into the Formik and add validation

```jsx
<Formik
  initialValues={{title:'',desc:'',address:'',price:'',image:''}}
  onSubmit={value=>onSubmitMethod(value)}
  validate={(values)=>{
      const errors={}
      if(!values.title)
      {
        console.log("title is not present");
        ToastAndroid.show("title must be there", ToastAndroid.SHORT)
        errors.name="Title must be there"   
      }
      return errors
  }}
```