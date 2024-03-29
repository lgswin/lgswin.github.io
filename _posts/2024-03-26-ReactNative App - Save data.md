---
layout: post
title: "ReactNative app #6 : Save data to firebase storage"
date: 2024-03-26
categories: [ReactNative, Firebase storage]
tags: [ReactNative, save data, firebase, storage]
---

To save data, first we need to upload an image into the firebase.

https://firebase.google.com/docs/storage/web/upload-files

Before uploading the image, we need to convert image to the blob file.

A "blob" file, short for "Binary Large Object," is a type of data storage format used to store binary data as a single entity. Blobs are typically used to store large objects such as images, videos, audio files, and other multimedia data, as well as binary data like executables or documents.

```jsx
const onSubmitMethod=async(value)=>{
    value.image=image;
    console.log(value);
    // Conver Uri to Blob File
    const resp=await fetch(image);
    const blob=await resp.blob();
}
```

- need to live Storage in firebase console

![s1](https://github.com/lgswin/lgswin.github.io/assets/83533586/451115e7-61a7-4b5c-9203-8f2959385dd0)

- need to change rule as true

![s2](https://github.com/lgswin/lgswin.github.io/assets/83533586/3db38b1d-02b3-4662-b17c-2c8a6de8b523)

- Initialization of storage

```jsx
import {getStorage, ref, uploadBytes} from 'firebase/storage';

export default function AddPostScreen() {
	const storage = getStorage();
```

- upload file

```jsx
const onSubmitMethod=async(value)=>{
    value.image=image;
    // Conver Uri to Blob File
    const resp=await fetch(image);
    const blob=await resp.blob();

    const storageRef = ref(storage, 'communityPost/'+Date.now()+".jpg");
    uploadBytes(storageRef, blob).then((snapshot) => {
        console.log('Uploaded a blob or file!');
    }).then((resp)=>{
        getDownloadURL(storageRef).then(async(downloadUrl)=> {
            console.log(downloadUrl);
						value.image = downloadUrl;
						// Add data 
            const docRef=await addDoc(collection(db,"UserPost"),value)
            if(docRef.id)
            {
                console.log("Document Added!")
            }
        })
    })
}
```

- add more information like name, email, imageUrl

```jsx

initialValues={{title:'',desc:'',address:'',price:'',image:'',userName:'',userEmail:'',userImage:""}}
```

```jsx

import { useUser } from '@clerk/clerk-expo';

...
const {user} = useUser();
    
... 
.then((resp)=>{
    getDownloadURL(storageRef).then(async(downloadUrl)=> {
        console.log(downloadUrl);
        value.image = downloadUrl;
        value.userName = user.fullName;
        value.userEmail = user.primaryEmailAddress.emailAddress;
        value.userImage = user.imageUrl;
```

- add loading

```jsx
const [loading,setLoading] = useState(false);
```

```jsx
const onSubmitMethod=async(value)=>{
    setLoading(true);
    ...

    const docRef=await addDoc(collection(db,"UserPost"),value)
    if(docRef.id)
    {
        setLoading(false);
        Alert.alert('Success!!','Post Added successfully')
    }
```

- add view component in submit button

```jsx
<TouchableOpacity onPress={handleSubmit} 
    style={{
        backgroundColor:loading?'#ccc':'#007BFF',
    }}
    disabled={loading}
    className="p-4 bg-blue-500 rounded-full mt-10">
{loading?
    <ActivityIndicator color='#fff' />
    :
    <Text className="text-white text-center text-[16px]">Submit</Text>
}
</TouchableOpacity>
```

<img width="300" alt="s3" src="https://github.com/lgswin/lgswin.github.io/assets/83533586/1703e8de-eb03-404c-93df-836c25f2115d">

in firebase console

![s4](https://github.com/lgswin/lgswin.github.io/assets/83533586/c662a641-2623-42f9-afc9-9562ef8fcb42)