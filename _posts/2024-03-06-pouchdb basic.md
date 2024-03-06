---
layout: post
title: "pouchdb"
date: 2024-03-06
categories: [PouchDB, basic]
tags: [pouchdb, database]
---

PouchDB is an open-source JavaScript database inspired by Apache CouchDB that is designed to run well within the browser.

## 1. setup and make database
```javascript
npm install --save pouchdb
```

if browser only?
```javascript
npm install --save pouchdb-browser
```

```javascript
<script src="//cdn.jsdelivr.net/npm/pouchdb@8.0.1/dist/pouchdb.min.js"></script>
<script>
  var db = new PouchDB('my_database');
</script>
```

## 2. include the module
```javascript
var PouchDB = require('pouchdb');
var db = new PouchDB('my_database');
```
or,
```javascript
var PouchDB = require('pouchdb-browser');
var db = new PouchDB('my_database');
```

## 3. create db and data
```javascript
const db = new PouchDB('users');

const doc = {
    _id: 'DaveInfo',
    name: 'Dave',
    age: 23,
    occupation: 'designer'
};

db.put(doc).then((res) => {
    console.log("Document inserted OK");
}).catch((err) => {
    console.error(err);
});
```

## 4. read data
```javascript
const db = new PouchDB('users');

db.get("PeterInfo").then((doc) => {
    console.log(doc);
}).catch((err) => {
    console.error(err);
});
```

## 5. update data
```javascript
const db = new PouchDB('users');

db.get("PeterInfo").then((doc) => {
    doc.occupation = "UX Designer";
    doc.age = "103";
    return db.put(doc);
}).then(() => {
    return db.get("PeterInfo");
}).then((doc) => {
    console.log(doc);
}).catch((err) => {
    console.error(err);
});
```

## 6. delete data
```javascript
const db = new PouchDB('users');

db.get("PeterInfo").then((doc) => {
    return db.remove(doc);
}).then((res) => {
    console.log(`The document has been removed ${JSON.stringify(res)}`);
}).catch((err) => {
    console.error(err);
});
```