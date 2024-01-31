
# 学习three.js之路 突破自己

## Three.js资源

github链接 : <a href="https://github.com/mrdoob/three.js">https://github.com/mrdoob/three.js</a>

Three.js官网 : <a href="https://github.com/mrdoob/three.js">https://threejs.org/</a>

### Threejs中文文档

<a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html">Threejs中文文档</a>

### Three.js-master文件下载

github下载threejs比较慢，所以在网盘放了一份，方便大家下载。

下载地址查看文章：<a href="http://www.yanhuangxueyuan.com/links.html">Three.js-master包下载</a>

<a href="https://github.com/chandlerprall/Physijs">Physijs</a>	Physijs是一款物理引擎，可以协助基于原生WebGL或使用three.js创建模拟物理现象，比如重力下落、物体碰撞等物理现
<a href="https://github.com/mrdoob/stats.js">stats.js</a>	JavaScript性能监控器，同样也可以测试webgl的渲染性能
<a href="https://github.com/dataarts/dat.gui">dat.gui</a>	轻量级的icon形用户界面框架，可以用来控制Javascript的变量，比如WebGL中一个物体的尺寸、颜色
<a href="https://github.com/tweenjs/tween.js/">tween.js</a>	借助tween.js快速创建补间动画，可以非常方便的控制机械、游戏角色运动
<a href="https://github.com/sshirokov/ThreeBSP">ThreeBSP</a>	可以作为three.js的插件，完成几何模型的布尔，各类三维建模软件基本都有布尔的概念


## 个人知识整理

### Threejs第一个3D场景

这是我学习的第一个创建立方体的例子  尽管代码少但是完整实现了一个世界的三维效果图

#### 案例源码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>第一个three.js文件_WebGL三维场景</title>
  <style>
    body {
      margin: 0;
      overflow: hidden;
      /* 隐藏body窗口区域滚动条 */
    }
  </style>
  <!--引入three.js三维引擎-->
  <script src="http://www.yanhuangxueyuan.com/versions/threejsR92/build/three.js"></script>
  <!-- <script src="http://www.yanhuangxueyuan.com/threejs/build/three.js"></script> -->
</head>

<body>
  <script>
    /**
     * 创建场景对象Scene
     */
    var scene = new THREE.Scene();
    /**
     * 创建网格模型
     */
    // var geometry = new THREE.SphereGeometry(60, 40, 40); //创建一个球体几何对象
    var geometry = new THREE.BoxGeometry(100, 100, 100); //创建一个立方体几何对象Geometry
    var material = new THREE.MeshLambertMaterial({
      color: 0x0000ff
    }); //材质对象Material
    var mesh = new THREE.Mesh(geometry, material); //网格模型对象Mesh
    scene.add(mesh); //网格模型添加到场景中
    /**
     * 光源设置
     */
    //点光源
    var point = new THREE.PointLight(0xffffff);
    point.position.set(400, 200, 300); //点光源位置
    scene.add(point); //点光源添加到场景中
    //环境光
    var ambient = new THREE.AmbientLight(0x444444);
    scene.add(ambient);
    // console.log(scene)
    // console.log(scene.children)
    /**
     * 相机设置
     */
    var width = window.innerWidth; //窗口宽度
    var height = window.innerHeight; //窗口高度
    var k = width / height; //窗口宽高比
    var s = 200; //三维场景显示范围控制系数，系数越大，显示的范围越大
    //创建相机对象
    var camera = new THREE.OrthographicCamera(-s * k, s * k, s, -s, 1, 1000);
    camera.position.set(200, 300, 200); //设置相机位置
    camera.lookAt(scene.position); //设置相机方向(指向的场景对象)
    /**
     * 创建渲染器对象
     */
    var renderer = new THREE.WebGLRenderer();
    renderer.setSize(width, height);//设置渲染区域尺寸
    renderer.setClearColor(0xb9d3ff, 1); //设置背景颜色
    document.body.appendChild(renderer.domElement); //body元素中插入canvas对象
    //执行渲染操作   指定场景、相机作为参数
    renderer.render(scene, camera);
  </script>
</body>
</html>
```

#### .html文件引入three.js引擎
在.html文件中引入three.js就像前端经常使用的jquery.js一样引入即可。

```js
  <!--引入three.js三维引擎-->
  <script src="http://www.yanhuangxueyuan.com/versions/threejsR92/build/three.js"></script>
```

#### 几何体Geometry
通过构造函数 THREE.BoxGeometry()创建了一个长宽高都是100的立方体
```js
//创建一个立方体几何对象Geometry
var geometry = new THREE.BoxGeometry(100, 100, 100);
```

通过构造函数 THREE.SphereGeometry()渲染出来一个球体效果
```js
//创建一个球体几何对象
var geometry = new THREE.SphereGeometry(60, 40, 40);
```

#### 材质Material
代码var material=new THREE.MeshLambertMaterial({color:0x0000ff});通过构造函数THREE.MeshLambertMaterial()创建了一个可以用于立方体的材质对象， 构造函数的参数是一个对象，对象包含了颜色、透明度等属性，本案例中只定义了颜色color;这里使用的是常见的非光泽表面没有镜面反射的的材料，我个人称它为暗光,那对立的就有亮光。
<table>
<thead>
<tr>
<th style="text-align:left">材质类型</th>
<th style="text-align:left">功能</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/materials/MeshBasicMaterial">MeshBasicMaterial</a></td>
<td style="text-align:left">基础网格材质，不受光照影响的材质</td>
</tr>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/materials/MeshLambertMaterial">MeshLambertMaterial</a></td>
<td style="text-align:left">Lambert网格材质，与光照有反应，漫反射</td>
</tr>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/materials/MeshPhongMaterial">MeshPhongMaterial</a></td>
<td style="text-align:left">高光Phong材质,与光照有反应</td>
</tr>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/materials/MeshStandardMaterial">MeshStandardMaterial</a></td>
<td style="text-align:left">PBR物理材质，相比较高光Phong材质可以更好的模拟金属、玻璃等效果</td>
</tr>
</tbody>
</table>

#### 光源Light
光源是给页面颜色的关键，不然会全部变黑。 常用的光源有 环境光和平行光 环境光又可以分为户外光照光 和场景基础光，两者都不会产生阴影  户外光照光又可以分为天上和地面发出的光。平行光的话 常见就是模拟太阳 。
<table>
<thead>
<tr>
<th style="text-align:left">光源</th>
<th style="text-align:left">简介</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/lights/AmbientLight">AmbientLight</a></td>
<td style="text-align:left">环境光</td>
</tr>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/lights/PointLight">PointLight</a></td>
<td style="text-align:left">点光源</td>
</tr>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/lights/DirectionalLight">DirectionalLight</a></td>
<td style="text-align:left">平行光，比如太阳光</td>
</tr>
<tr>
<td style="text-align:left"><a href="http://www.yanhuangxueyuan.com/threejs/docs/index.html#api/zh/lights/SpotLight">SpotLight</a></td>
<td style="text-align:left">聚光源</td>
</tr>
</tbody>
</table>
#### 相机Camera
相机的话 你可以认为是视角 ，三个关键 位置,视线方向,投影方式 ,camera.position.set(200, 300, 200);和camera.lookAt(scene.position);定义的是相机的位置和拍照方向，也就是位置和视线方向，投影方式的话分为投射投影（PerspectiveCamera）模拟人类视角 和正射投影（OrthographicCamera）

#### 场景-相机-渲染器


<p><embed src="http://www.webgl3d.cn/upload/threejs9%E7%BB%93%E6%9E%84.svg">

</p>

从实际生活中拍照角度或是使用三维渲染软件角度理解本节课的案例代码，立方体网格模型和光照组成了一个虚拟的三维场景,相机对象就像你生活中使用的相机一样可以拍照，只不过一个是拍摄真实的景物，一个是拍摄虚拟的景物，拍摄一个物体的时候相机的位置和角度需要设置，虚拟的相机还需要设置投影方式，当你创建好一个三维场景，相机也设置好，就差一个动作“咔”，通过渲染器就可以执行拍照动作。

### 旋转动画、requestAnimationFrame周期性渲染

#### 周期性渲染
按照一定的周期不停地调用渲染方法.render()就可以不停地生成新的图像覆盖原来的图像。这也就是说只要一边旋转立方体，一边执行渲染方法.render()重新渲染，就可以实现立方体的旋转效果。
为了实现周期性渲染可以通过浏览器全局对象window对象的一个方法setInterval(),可以通过window对象调用该方法window.setInterval()，也可以直接以函数形式调用setInterval()。

```js
// 间隔20ms周期性调用函数fun
setInterval("render()",20)

// 渲染函数
function render() {
    renderer.render(scene,camera);//执行渲染操作
    mesh.rotateY(0.01);//每次绕y轴旋转0.01弧度
}
//间隔20ms周期性调用函数fun,20ms也就是刷新频率是50FPS(1s/20ms)，每秒渲染50次
setInterval("render()",20);
```

#### 渲染频率
调用渲染方法.render()进行渲染的渲染频率不能太低，比如执行setInterval("render()",200);间隔200毫秒调用渲染函数渲染一次，相当于每秒渲染5次，你会感觉到比较卡顿。渲染频率除了不能太低，也不能太高，太高的话计算机的硬件资源跟不上，函数setInterval()设定的渲染方式也未必能够正常实现。一般调用渲染方法.render()进行渲染的渲染频率控制在每秒30~60次，人的视觉效果都很正常，也可以兼顾渲染性能。

```js
//设置调用render函数的周期为200ms，刷新频率相当于5你能明显的感受到卡顿
setInterval("render()",200);
```

#### 函数requestAnimationFrame()
前面讲解threejs动画效果，使用了setInterval()函数，实际开发中，为了更好的利用浏览器渲染，可以使用函数requestAnimationFrame()代替setInterval()函数，requestAnimationFrame()和setInterval()一样都是浏览器window对象的方法。

requestAnimationFrame()参数是将要被调用函数的函数名，requestAnimationFrame()调用一个函数不是立即调用而是向浏览器发起一个执行某函数的请求， 什么时候会执行由浏览器决定，一般默认保持60FPS的频率，大约每16.7ms调用一次requestAnimationFrame()方法指定的函数，60FPS是理想的情况下，如果渲染的场景比较复杂或者说硬件性能有限可能会低于这个频率

```js
function render() {
        renderer.render(scene,camera);//执行渲染操作
        mesh.rotateY(0.01);//每次绕y轴旋转0.01弧度
        requestAnimationFrame(render);//请求再次执行渲染函数render
    }
render();
```

# 实例

## three.js 飞机

### 人物驾驶者飞机

#### 人物

##### 一. 首先是身体位置
```js
var bodyGeom = new THREE.BoxGeometry(15,15,15);
var bodyMat = new THREE.MeshPhongMaterial({color:Colors.brown, shading:THREE.FlatShading});
var body = new THREE.Mesh(bodyGeom, bodyMat);
body.position.set(2,-12,0);

this.mesh.add(body);
```
##### 二. 然后是脸的位置
```js
var faceGeom = new THREE.BoxGeometry(10,10,10);
var faceMat = new THREE.MeshLambertMaterial({color:Colors.pink});
var face = new THREE.Mesh(faceGeom, faceMat);
this.mesh.add(face);
```

<img src="https://img.zhangaming.com/three/img1.png">

##### 三. 完善好脸部和头发

3.1 建立头部基类
var hairs = new THREE.Object3D();
底部发型
```js
var hairGeom = new THREE.BoxGeometry(4,4,4);
var hairMat = new THREE.MeshLambertMaterial({color:Colors.brown});
var hair = new THREE.Mesh(hairGeom, hairMat); //创建头发模型
hair.geometry.applyMatrix(new THREE.Matrix4().makeTranslation(0,2,0)); //矩阵操作 改变形状
var hairs = new THREE.Object3D();
```
生成顶部头发

```js
for (var i=0; i<12; i++){
  var h = hair.clone();
  var col = i%3;
  var row = Math.floor(i/3);
  var startPosZ = -4;
  var startPosX = -4;
  h.position.set(startPosX + row*4, 0, startPosZ + col*4);
  this.hairsTop.add(h);
}
hairs.add(this.hairsTop);
```
左右两侧的头发
```js
var hairSideGeom = new THREE.BoxGeometry(12,4,2);
hairSideGeom.applyMatrix(new THREE.Matrix4().makeTranslation(-6,0,0));
var hairSideR = new THREE.Mesh(hairSideGeom, hairMat);
var hairSideL = hairSideR.clone();
hairSideR.position.set(8,-2,6);
hairSideL.position.set(8,-2,-6);
hairs.add(hairSideR);
hairs.add(hairSideL);
```
头后部头发
```js
var hairBackGeom = new THREE.BoxGeometry(2,8,10);
var hairBack = new THREE.Mesh(hairBackGeom, hairMat);
hairBack.position.set(-1,-4,0)
hairs.add(hairBack);
hairs.position.set(-5,5,0);
```
<img src="https://img.zhangaming.com/three/hair1.png">
<br>
<img src="https://img.zhangaming.com/three/hair2.png">

挡风眼睛：两个镜框+绑带
```js
var glassGeom = new THREE.BoxGeometry(5,5,5);
var glassMat = new THREE.MeshLambertMaterial({color:Colors.brown});
var glassR = new THREE.Mesh(glassGeom,glassMat);
glassR.position.set(6,0,3);
var glassL = glassR.clone();
glassL.position.z = -glassR.position.z

var glassAGeom = new THREE.BoxGeometry(11,1,11);
var glassA = new THREE.Mesh(glassAGeom, glassMat);
this.mesh.add(glassR);
this.mesh.add(glassL);
this.mesh.add(glassA);
```
<img src="https://img.zhangaming.com/three/glass.png">
<br>

左右耳
```js
var earGeom = new THREE.BoxGeometry(2,3,2);
var earL = new THREE.Mesh(earGeom,faceMat);
earL.position.set(0,0,-6);
var earR = earL.clone();
earR.position.set(0,0,6);
this.mesh.add(earL);
this.mesh.add(earR);
```
<img src="https://img.zhangaming.com/three/ear1.png">
<br>
<img src="https://img.zhangaming.com/three/ear2.png">

<!-- 让头发动起来 -->
```js
Pilot.prototype.updateHairs = function(){
var hairs = this.hairsTop.children;

var l = hairs.length;
for (var i=0; i<l; i++){
  var h = hairs[i];
  h.scale.y = .75 + Math.cos(this.angleHairs+i/3)*.25; //因为头发是4*3的方块 按角度给头发高度伸缩
}
this.angleHairs += 0.16;
}

pilot.updateHairs();
//调用
```
人物弄完了 然后是飞机

#### 飞机的建造

##### 机身

```js
var geomCockpit = new THREE.BoxGeometry(80,50,50,1,1,1);
var matCockpit = new THREE.MeshPhongMaterial({color:Colors.red, shading:THREE.FlatShading});
var cockpit = new THREE.Mesh(geomCockpit, matCockpit);
cockpit.castShadow = true;
cockpit.receiveShadow = true;
this.mesh.add(cockpit);
```
##### 引擎
```js
var geomEngine = new THREE.BoxGeometry(20,50,50,1,1,1);
var matEngine = new THREE.MeshPhongMaterial({color:Colors.white, shading:THREE.FlatShading});
var engine = new THREE.Mesh(geomEngine, matEngine);
engine.position.x = 50;
engine.castShadow = true;
engine.receiveShadow = true;
this.mesh.add(engine);
```
<img src="https://img.zhangaming.com/three/Cockpit.png">
<br>

##### 尾翼

```js
var geomTailPlane = new THREE.BoxGeometry(15,20,5,1,1,1); //长宽高 +段数
var matTailPlane = new THREE.MeshPhongMaterial({color:Colors.red, shading:THREE.FlatShading});
var tailPlane = new THREE.Mesh(geomTailPlane, matTailPlane);
tailPlane.position.set(-40,20,0);
tailPlane.castShadow = true;
tailPlane.receiveShadow = true;
this.mesh.add(tailPlane);
```
##### 机翼

```js
var geomSideWing = new THREE.BoxGeometry(30,5,120,1,1,1);
var matSideWing = new THREE.MeshPhongMaterial({color:Colors.red, shading:THREE.FlatShading});
var sideWing = new THREE.Mesh(geomSideWing, matSideWing);
sideWing.position.set(0,15,0);
sideWing.castShadow = true;
sideWing.receiveShadow = true;
this.mesh.add(sideWing);
```
<img src="https://img.zhangaming.com/three/wings.png">
<br>

##### 挡风玻璃 +透明属性
```js
var geomWindshield = new THREE.BoxGeometry(3,15,20,1,1,1);
var matWindshield = new THREE.MeshPhongMaterial({color:Colors.white,transparent:true, opacity:.3, shading:THREE.FlatShading});;
var windshield = new THREE.Mesh(geomWindshield, matWindshield);
windshield.position.set(5,27,0);

windshield.castShadow = true;
windshield.receiveShadow = true;

this.mesh.add(windshield);
```
<img src="https://img.zhangaming.com/three/windshield.png">
<br>

##### 螺旋桨支架
```js
var geomPropeller = new THREE.BoxGeometry(20,10,10,1,1,1);
var matPropeller = new THREE.MeshPhongMaterial({color:Colors.brown, shading:THREE.FlatShading});
this.propeller = new THREE.Mesh(geomPropeller, matPropeller);

this.propeller.castShadow = true;
this.propeller.receiveShadow = true;
this.propeller.position.set(60,0,0);
this.mesh.add(this.propeller);
```
<img src="https://img.zhangaming.com/three/propeller.png">
<br>

##### 先对机身和 螺旋桨支架做变形 
找到之前写机身的螺旋桨支架的代码  对BoxGeometry几何的顶点做修改
机身的顶点参数
```js
// 0: Vector3 {x: 40, y: 25, z: 25}
// 1: Vector3 {x: 40, y: 25, z: -25}
// 2: Vector3 {x: 40, y: -25, z: 25}
// 3: Vector3 {x: 40, y: -25, z: -25}
// 4: Vector3 {x: -40, y: 15, z: -5}
// 5: Vector3 {x: -40, y: 15, z: 5}
// 6: Vector3 {x: -40, y: 5, z: -5}
// 7: Vector3 {x: -40, y: 5, z: 5}
geomCockpit.vertices[4].y-=10;
geomCockpit.vertices[4].z+=20;
geomCockpit.vertices[5].y-=10;
geomCockpit.vertices[5].z-=20;
geomCockpit.vertices[6].y+=30;
geomCockpit.vertices[6].z+=20;
geomCockpit.vertices[7].y+=30;
geomCockpit.vertices[7].z-=20;
```
螺旋桨支架的顶点参数
```js
// 0: Vector3 {x: 10, y: 5, z: 5}
// 1: Vector3 {x: 10, y: 5, z: -5}
// 2: Vector3 {x: 10, y: -5, z: 5}
// 3: Vector3 {x: 10, y: -5, z: -5}
// 4: Vector3 {x: -10, y: 5, z: -5}
// 5: Vector3 {x: -10, y: 5, z: 5}
// 6: Vector3 {x: -10, y: -5, z: -5}
// 7: Vector3 {x: -10, y: -5, z: 5}
geomPropeller.vertices[4].y-=5;
geomPropeller.vertices[4].z+=5;
geomPropeller.vertices[5].y-=5;
geomPropeller.vertices[5].z-=5;
geomPropeller.vertices[6].y+=5;
geomPropeller.vertices[6].z+=5;
geomPropeller.vertices[7].y+=5;
geomPropeller.vertices[7].z-=5;
```
<img src="https://img.zhangaming.com/three/boxchange.png">
<br>

##### 螺旋桨桨板

桨板使用两个长发体 做十
```js
var geomBlade = new THREE.BoxGeometry(1,80,10,1,1,1);
var matBlade = new THREE.MeshPhongMaterial({color:Colors.brownDark, shading:THREE.FlatShading});
var blade1 = new THREE.Mesh(geomBlade, matBlade);
blade1.position.set(8,0,0);

blade1.castShadow = true;
blade1.receiveShadow = true;

var blade2 = blade1.clone();
blade2.rotation.x = Math.PI/2;

blade2.castShadow = true;
blade2.receiveShadow = true;

this.propeller.add(blade1);
this.propeller.add(blade2);
```

<img src="https://img.zhangaming.com/three/blade.png">
<br>

##### 车身两侧装车轮的保护壳

```js
var wheelProtecGeom = new THREE.BoxGeometry(30,15,10,1,1,1);
var wheelProtecMat = new THREE.MeshPhongMaterial({color:Colors.red, shading:THREE.FlatShading});
var wheelProtecR = new THREE.Mesh(wheelProtecGeom,wheelProtecMat);
wheelProtecR.position.set(25,-20,25);
this.mesh.add(wheelProtecR);

var wheelProtecL = wheelProtecR.clone();
wheelProtecL.position.z = -wheelProtecR.position.z ;
this.mesh.add(wheelProtecL);
```
<img src="https://img.zhangaming.com/three/wheelProtec.png">
<br>

##### 两侧车轮 和尾翼车轮

```js
var wheelTireGeom = new THREE.BoxGeometry(24,24,4);
var wheelTireMat = new THREE.MeshPhongMaterial({color:Colors.brownDark, shading:THREE.FlatShading});
var wheelTireR = new THREE.Mesh(wheelTireGeom,wheelTireMat);
wheelTireR.position.set(25,-28,25);

var wheelAxisGeom = new THREE.BoxGeometry(10,10,6);
var wheelAxisMat = new THREE.MeshPhongMaterial({color:Colors.brown, shading:THREE.FlatShading});
var wheelAxis = new THREE.Mesh(wheelAxisGeom,wheelAxisMat);
wheelTireR.add(wheelAxis);

this.mesh.add(wheelTireR); //右边

var wheelTireL = wheelTireR.clone();
wheelTireL.position.z = -wheelTireR.position.z;
this.mesh.add(wheelTireL); //左边

var wheelTireB = wheelTireR.clone();
wheelTireB.scale.set(.5,.5,.5);
wheelTireB.position.set(-35,-5,0);
this.mesh.add(wheelTireB); //后面
```
<img src="https://img.zhangaming.com/three/wheel.png">
<br>

##### 尾部悬挂支架

```js
var suspensionGeom = new THREE.BoxGeometry(4,20,4);
suspensionGeom.applyMatrix(new THREE.Matrix4().makeTranslation(0,10,0))
var suspensionMat = new THREE.MeshPhongMaterial({color:Colors.red, shading:THREE.FlatShading});
var suspension = new THREE.Mesh(suspensionGeom,suspensionMat);
suspension.position.set(-35,-5,0);
suspension.rotation.z = -.3;
this.mesh.add(suspension);
```
<img src="https://img.zhangaming.com/three/suspension.png">
<br>


#### 组装 飞行员和飞机
只要把飞行员 加到飞机的模型里 然后调整位置就行
```js
// var AirPlane = function(){
this.mesh = new THREE.Object3D();
this.mesh.name = "airPlane";
...
this.pilot = new Pilot();
this.pilot.mesh.position.set(-10,27,0);
this.mesh.add(this.pilot.mesh);
```
<img src="https://img.zhangaming.com/three/airPlane.png">
<br>

#### 飞行支架转动

在循环渲染里去改变旋转角度就行
```js
function loop(){
 airplane.propeller.rotation.x += 0.3;
 renderer.render(scene, camera);
requestAnimationFrame(loop);
}
```

### 天空与云

#### 先造一片云
建一块20*20*20的云 然后3-6块连在一起
位置和旋转角度随机  缩放也是随机 然后添加到云的模型基类里
```js
Cloud = function(){
this.mesh = new THREE.Object3D();
this.mesh.name = "cloud";
var geom = new THREE.CubeGeometry(20,20,20);
var mat = new THREE.MeshPhongMaterial({
  color:Colors.white,
});

var nBlocs = 3+Math.floor(Math.random()*3);
for (var i=0; i<nBlocs; i++ ){
  var m = new THREE.Mesh(geom.clone(), mat);
  m.position.x = i*15;
  m.position.y = Math.random()*10;
  m.position.z = Math.random()*10;
  m.rotation.z = Math.random()*Math.PI*2;
  m.rotation.y = Math.random()*Math.PI*2;
  var s = .1 + Math.random()*.9;
  m.scale.set(s,s,s);
  m.castShadow = true;
  m.receiveShadow = true;
  this.mesh.add(m);
}
}


```

#### 创建天空类
创建20块存放云的地方 以360度 为范围

高度范围有个区间值  角度就20个位置  然后缩放随机
```js
this.mesh = new THREE.Object3D();
this.nClouds = 20;
this.clouds = [];
var stepAngle = Math.PI*2 / this.nClouds;
for(var i=0; i<this.nClouds; i++){
  var c = new Cloud();
  this.clouds.push(c);
  var a = stepAngle*i;
  var h = 750 + Math.random()*200;
  c.mesh.position.y = Math.sin(a)*h;
  c.mesh.position.x = Math.cos(a)*h;
  c.mesh.position.z = -400-Math.random()*400;
  c.mesh.rotation.z = a + Math.PI/2;
  var s = 1+Math.random()*2;
  c.mesh.scale.set(s,s,s);
  this.mesh.add(c.mesh);
}
}

```

#### 天空云层旋转
跟螺旋桨一样
在循环渲染里去改变旋转角度就行
```js
function loop(){
  sky.mesh.rotation.z += .01;
 renderer.render(scene, camera);
requestAnimationFrame(loop);
}
```

<img src="https://img.zhangaming.com/three/sky.png">
<br>

### 创建大海

海的话 其实的一个圆柱体 横着放  然后控制段数 形成波浪
```js
Sea = function(){
var geom = new THREE.CylinderGeometry(600,600,800,40,10); //圆柱体几何体 顶部圆半径 底部圆半径高度 圆截面分成多少段 竖直方向上分成多少段
//旋转X轴-90
geom.applyMatrix(new THREE.Matrix4().makeRotationX(-Math.PI/2));
// 合并 顶点
geom.mergeVertices();
var l = geom.vertices.length;

this.waves = [];

for (var i=0;i<l;i++){
  var v = geom.vertices[i];
  this.waves.push({y:v.y,
                   x:v.x,
                   z:v.z,
                   ang:Math.random()*Math.PI*2,
                   amp:5 + Math.random()*15,
                   speed:0.016 + Math.random()*0.032
                  });
};
// 海的材质
var mat = new THREE.MeshPhongMaterial({
  color:Colors.blue,
  transparent:true,
  opacity:.8,
  shading:THREE.FlatShading,

});

this.mesh = new THREE.Mesh(geom, mat);
this.mesh.receiveShadow = true;
}

Sea.prototype.moveWaves = function (){
var verts = this.mesh.geometry.vertices;
var l = verts.length;
for (var i=0; i<l; i++){
  var v = verts[i];
  var vprops = this.waves[i];
  v.x =  vprops.x + Math.cos(vprops.ang)*vprops.amp;
  v.y = vprops.y + Math.sin(vprops.ang)*vprops.amp;
  vprops.ang += vprops.speed;
}
// three.js vertices 我要更新了 在 更新 verties 的信息有用
this.mesh.geometry.verticesNeedUpdate=true;
sea.mesh.rotation.z += .005;
}
```

#### 大海转动

一样在循环渲染里 去调用
```js
function loop(){
  sea.moveWaves();
 renderer.render(scene, camera);
reque
```