---
title: 1_Three开始
date: 2023-2-20
description: 为了准备情侣宠物，简单学习了一下Three.js的使用
tags:
  - 学习
categories: Js库
---

> 官方文档：https://threejs.org/
>
> 中文文档：https://www.three3d.cn/

# 一、Vue+Three.js的环境搭建

1. 创建Vue项目

2. 在vue文件中引入Three

   ```javascript
   import * as THREE from 'three'
   ```

3. 在methods中加入initThree()方法

   ```javascript
   methods: {
       initThree() {
   
       }
   }
   ```

4. 写代码！！！

# 二、入门程序

1. 创建场景

   ```javascript
   const scene = new THREE.Scene();
   ```

2. 创建照相机

   ```javascript
   // 使用PerspectiveCamera（透视摄像机）
   const camera = new THREE.PerspectiveCamera(
   	75, // 视野角度（FOV）
       window.innerWidth / window.innerHeight, // 长宽比（aspect ratio）
       0.1, // 近截面
       1000 // 远截面
   );
   ```

3. 设置照相机的位置并添加到场景中

   ```javascript
   camera.position.set(0, 0, 10);
   scene.add(camera);
   ```

4. 添加物体并创建材质

   ```javascript
   const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
   const cubeMaterial = new THREE.MeshBasicMaterial({color: 0xffff00});
   ```

5. 根据几何体和材质创建物体并添加到场景中

   ```javascript
   const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
   scene.add(cube);
   ```

6. 初始化渲染器并设置渲染器尺寸大小

   ```javascript
   const renderer = new THREE.WebGLRenderer();
   renderer.setSize(window.innerWidth, window.innerHeight);
   ```

7. 将webgl渲染的canvas内容添加到body中

   ```javascript
   document.body.appendChild(renderer.domElement);
   ```

8. 使用渲染器，通过照相机将场景渲染进来

   ```javascript
   renderer.render(scene, camera);
   ```

   