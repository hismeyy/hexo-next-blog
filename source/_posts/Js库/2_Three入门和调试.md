---
title: 2_Three入门和调试
date: 2023-2-21
description: 为了准备情侣宠物，简单学习了一下Three.js的使用
tags:
  - 学习
categories: Js库
---

# 一、轨道控制器查看物体

1. 导入轨道控制器

   ```javascript
   import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
   ```

2. 创建轨道控制器

   ```javascript
   const controls = new OrbitControls(camera, renderer.domElement);
   ```

   传入的参数

   - 相机：让相机围绕目标运动，默认目标是原点
   - 渲染的画布Dom对象，用于监听鼠标事件控制相机围绕运动

3. 每一帧根据控制器更新画面

   ```javascript
   function render() {
     //如果后期需要控制器带有阻尼效果，或者自动旋转等效果，就需要加入controls.update()
     controls.update()
     renderer.render(scene, camera);
     //   渲染下一帧的时候就会调用render函数
     requestAnimationFrame(render);
   }
   
   render();
   ```


# 二、控制物体移动

1. 每一帧修改一点位置形成动画

   ```javascript
   function render() {
     cube.position.x += 0.01;
     if (cube.position.x > 5) {
       cube.position.x = 0;
     }
     renderer.render(scene, camera);
     //   渲染下一帧的时候就会调用render函数
     requestAnimationFrame(render);
   }
   
   render();
   ```

# 三、添加坐标轴辅助器

1. 坐标轴辅助器

   ```javascript
   const axesHelper = new THREE.AxesHelper( 5 );
   scene.add( axesHelper );
   ```

2. ArrowHelper箭头辅助器

   ```javascript
   const dir = new THREE.Vector3( 1, 2, 0 );
   
   //normalize the direction vector (convert to vector of length 1)
   dir.normalize();
   
   const origin = new THREE.Vector3( 0, 0, 0 );
   const length = 1;
   const hex = 0xffff00;
   
   const arrowHelper = new THREE.ArrowHelper( dir, origin, length, hex );
   scene.add( arrowHelper )
   ```

# 四、物体缩放和旋转

1. scale设置缩放

   ```javascript
   // 设置x轴放大3倍、y轴方向放大2倍、z轴方向不变
   cube.scale.set(3, 2, 1);
   // 单独设置缩放
   cube.scale.x = 3
   ```

2. rotation设置旋转

   ```javascript
   // 直接设置旋转属性，例如围绕x轴旋转90度
   cube.rotation.x = -Math.PI/2
   // 围绕x轴旋转45度
   cube.rotation.set(-Math.PI / 4, 0, 0, "XZY");
   ```

# 五、正确处理动画运动

- 请求动画帧间隔不固定

```javascript
function render(time) {
  let t = (time / 1000) % 5;
  cube.position.x = t * 1;
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}
```

# 六、Clock跟踪时间处理动画

```javascript
// 获取时钟运行的总时长
let time = clock.getElapsedTime();
console.log("时钟运行总时长：", time);
// 两次获取时间的间隔时间
let deltaTime = clock.getDelta();
console.log("两次获取时间的间隔时间：", deltaTime);
let t = time % 5;
cube.position.x = t * 1;
```

# 七、GSAP模块的使用

1. 安装

   ```cmd
   yarm add gsap
   ```

2. 导入

   ```javascript
   import gsap from "gsap";
   ```

# 八、自适应屏幕大小

1. 监听屏幕变化

   ```javascript
   // 监听画面变化，更新渲染画面
   window.addEventListener("resize", () => {
     //   console.log("画面变化了");
     // 更新摄像头
     camera.aspect = window.innerWidth / window.innerHeight;
     // 更新摄像机的投影矩阵
     camera.updateProjectionMatrix();
   
     // 更新渲染器
     renderer.setSize(window.innerWidth, window.innerHeight);
     // 设置渲染器的像素比
     renderer.setPixelRatio(window.devicePixelRatio);
   });
   ```

2. 控制场景全屏

   ```javascript
   window.addEventListener("dblclick", () => {
     const fullScreenElement = document.fullscreenElement;
     if (!fullScreenElement) {
       //   双击控制屏幕进入全屏，退出全屏
       // 让画布对象全屏
       renderer.domElement.requestFullscreen();
     } else {
       //   退出全屏，使用document对象
       document.exitFullscreen();
     }
   });
   ```
