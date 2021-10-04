<template>
  <div id="wrapper">
    <main>
      <div class="left-side">
        <span class="title">999
        </span>
        <system-information></system-information>
      </div>

      <div class="right-side">
        <div class="doc">
          <div class="title">Getting Started</div>
          <p>
            electron-vue comes packed with detailed documentation that covers everything from
            internal configurations, using the project structure, building your application,
            and so much more.
          </p>
          <button @click="open('https://simulatedgreg.gitbooks.io/electron-vue/content/')">Read the Docs</button><br><br>
        </div>
        <div class="doc">
          <div class="title alt">Other Documentation</div>
          <button class="alt" @click="open('https://electron.atom.io/docs/')">Electron</button>
          <button class="alt" @click="open('https://vuejs.org/v2/guide/')">Vue.js</button>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
  import SystemInformation from './LandingPage/SystemInformation'
  import * as THREE from 'three';
  import {OrbitControls} from "three/examples/jsm/controls/OrbitControls";
  export default {
    name: 'landing-page',
    components: { SystemInformation },
    methods: {
      open(link){
        window.open('https://www.baidu.com/');
        //  this.$router.push('https://www.baidu.com/')
        //  window.location.href="https://www.baidu.com/"
        // window.open(link);
        // this.$electron.shell.openExternal(link)
      },
      init() {
        // 创建场景
        var scene = new THREE.Scene();
        // 设置摄像机
        var camera = new THREE.PerspectiveCamera(45, window.innerWidth / window.innerHeight, 0.1, 2000)
        // 创建渲染器
        var renderer = new THREE.WebGLRenderer();
        // 设置渲染器的初始颜色
        renderer.setClearColor(new THREE.Color(0xeeeeee));
        // 设置输出canvas画面的大小
        renderer.setSize(window.innerWidth, window.innerHeight)
        // 设置渲染物体阴影
        renderer.shadowMapEnabled = true;
        // 显示三维坐标系
        var axes = new THREE.AxisHelper(20)


        // 添加坐标系到场景中
        scene.add(axes);
        // 创建地面的几何体
        var planeGeometry = new THREE.PlaneGeometry(60, 20);
        // 给地面物体上色
        var planeMaterial = new THREE.MeshBasicMaterial({ color: 0xcccccc });
        // 创建地面
        var plane = new THREE.Mesh(planeGeometry, planeMaterial)
        // 物体移动位置
        // plane.rotation.x = -0.5 * Math.PI;
        // plane.position.x = 15;
        plane.rotation.x = -0.5 * Math.PI;
        plane.position.x = 0;
        plane.position.y = 0;
        plane.position.z = 0;
        plane.castShadow = true;
        // 接收阴影
        plane.receiveShadow = true;

        // 将地面添加到场景中
        scene.add(plane);

        // 添加立方体
        var cubeGeometry = new THREE.BoxGeometry(4, 4, 4);
        var cubeMaterial = new THREE.MeshLambertMaterial({ color: 0xff0000 })
        var cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

        cube.position.x = 0;
        cube.position.y = 4;
        cube.position.z = 2;
        // 对象是否渲染到阴影贴图当中
        cube.castShadow = true;

        scene.add(cube)

        // 球体
        var sphereGeometry = new THREE.SphereGeometry(4, 20, 20);
        var spherMaterial = new THREE.MeshLambertMaterial({ color: 0xff0000 })
        var sphere = new THREE.Mesh(sphereGeometry, spherMaterial)
        sphere.position.x = 10;
        sphere.position.y = 4;
        sphere.position.z = 0;
        // 对象是否渲染到阴影贴图当中
        sphere.castShadow = true;

        scene.add(sphere)


        // 创建聚光灯
        var spotLight = new THREE.SpotLight(0xFFFFFF);
        spotLight.position.set(130, 130, -130);
        spotLight.castShadow = true;
        // 添加聚光灯
        scene.add(spotLight)

        // 定位相机，并且指向场景中心
        camera.position.x = 30;
        camera.position.y = 30;
        camera.position.z = 30;
        camera.lookAt(scene.position)

        // 将渲染器输出添加html元素中
        document.getElementById('wrapper').appendChild(renderer.domElement);
        renderer.render(scene, camera)
        var controls = new OrbitControls(camera, renderer.domElement)
        // 监听控制器的鼠标事件，执行渲染内容
        controls.addEventListener('change', () => {
          renderer.render(scene, camera)
        })
        let T0 = new Date()
        function render() {
          let T1 = new Date()
          let t = T1 - T0;
          T0 = T1;
          renderer.render(scene, camera)
          // 每次绕y轴旋转0.01弧度
          // 每一毫秒渲染0.001弧度
          cube.rotateY(0.001 * t)
          window.requestAnimationFrame(render)
        }
        // setInterval(render, 16)

        window.requestAnimationFrame(render)
      }
    },
    mounted(){
      console.log(88)
      this.init()
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  #wrapper {
    background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
    height: 100vh;
    /* padding: 60px 80px; */
    width: 100vw;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    display: flex;
    justify-content: space-between;
  }

  main > div { flex-basis: 50%; }

  .left-side {
    display: flex;
    flex-direction: column;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc p {
    color: black;
    margin-bottom: 10px;
  }

  .doc button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
</style>
