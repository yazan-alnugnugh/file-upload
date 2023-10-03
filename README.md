![npm](https://img.shields.io/npm/dm/@yazan.alnughnugh/file-upload)
![npm](https://img.shields.io/npm/v/@yazan.alnughnugh/file-upload)
[![StyleCI](https://github.styleci.io/repos/452773969/shield?branch=master)](https://github.styleci.io/repos/452773969?branch=master)

<h2 align="center">Supporting Upload file component</h2>

- [Become sponsor on Patreon](https://www.patreon.com/yazan_alnughnugh).
- [One-time donation via crypto-currencies](https://github.com/yazan-alnugnugh/laravel-datatable/blob/master/_docs/Donations/crypto/index.md).


# Introduction

<p align="center">
    <img src="art/intro-image.png" alt="laravel-vue-datatable intro image">
</p>

**Are you looking for an easy way to upload files, this is what are you looking for.**

This is a vue component for uploading files using ajax. It contain amazing features, you can use it easily without any complexity.
## Features
- Preview files before upload
- Multi-file upload
- Drag and Drop files
- Upload multiple files at the same time
- Delete/Delete all
- Cancel/Cancel all
- Chunk upload

## Official Documentation

 Documentation for File Upload can be found here  [here](https://packages.tourismcaravan.com/docs/1/file-upload)

## Demo

 [File Upload Demo](https://packages.tourismcaravan.com/uploads)
 
## Requirements

* [Tailwind](https://tailwindcss.com/) => 3.*
* [Axios](https://github.com/axios/axios) => 2.2.17*
* [Fontawesome](fontawesome.com) => 5.15*


## Installation
   
 
To install and setup the component we will follow a few steps:

#### step 1 
```bash
npm install `@yazan.alnughnugh/file-upload
```
#### step 2 

Vue 2
```javascript
// app/resources/js/app.js

 Vue.component('file-upload', require('@yazan.alnughnugh/file-upload').default);

```

Vue 3
```javascript
// app/resources/js/app.js
 import FileUpload from '@yazan.alnughnugh/file-upload';

 const app = createApp(options);

 app.component('file-upload', FileUpload);
 app.mount('#app');

```


## Usage
   
To start use the component we will add a component to your blade file with three props, 

   ```url``` is the url where you want to send request to it,
   
   ```id``` if you want to send model id with request,
   
   ```label``` here you can add your description.
   
```html
// resources/posts/create.blade.php

  <file-upload :url="url" :id="id" :label="label"></file-upload>

```

#### Request
The request will be the same as bellow, The files will be sent one By one , one request for each file.

```php
// resources/posts/create.blade.php

  [
    "id" => "9", // if you added id
    "file" => File , // this is our file
  ] ;   

```

## Configuration


| Name | Type | Default | Description  
| --- | --- | --- | --- |
| `url ` | String | "/" | is the url where you want to send request to it  |
| `id` | Number | null |  if you want to send model id with request |
| `label` | String | 'PNG, JPG, GIF up to 10MB' | here you can add your description |
