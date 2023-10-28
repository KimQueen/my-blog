---
title: antdesign的上传功能使用
categories: 前端功能实现代码
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 表格
  - antdsign
  - react
toc: true
thumbnail: "/images/tree.jpg"
---
我们要实现的是在点击上传按钮的时候，弹出一个对话框，在选择文件的之后点击完成的时候，向指定的地址上传文件

<!--more-->

![弹框](https://upload-images.jianshu.io/upload_images/13681871-3863bfa848ec03a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

html的相关代码
```
<!--html文件-->
<!--multiple为true可以同时上传多个文件-->
<!--showUploadList 为false不现实文件的上传情况-->
<Upload
  multiple ={true}
  accept ='.mib'
  beforeUpload = {this.uploadMibfiles}
  onChange ={this.fileState}
  showUploadList = {false}
/>
```
js的逻辑代码
```
// 将获取到的文件进行保存，方便后续进行操作
uploadMibfiles = file =>{
 this.fileList = file;
}

 fileState = info =>{
  if(this.fileListMy && this.fileListMy.length !== 0){
    // 写相关发请求的逻辑
    apiUp (this.fileList）.then(res =>{
      if  (res !== 202){
        message.success('文件上传成功')
      }else{
        message.errro("文件上传失败")
      }
    })
  }
}
```
发送请求的代码
```
const apiUp = async file =>{
  const formData = new FormData(); // 将文件变成键值的形式，可以方便的处理各类的文件
  formData .append('file',file);
  const res = await axios.post('/wdfedf/guyfewj',formDate);
  return res;
}
```