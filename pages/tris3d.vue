<template>
  <div>
    <canvas id="tris3DCanvas"></canvas>
    <button @click="init()">Ciao</button>
  </div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

export default {
  name: "tris3D",
  data() {
    return {};
  },
  mounted() {
    this.init();
    console.log("Mounted");
  },
  methods: {
    init() {
      console.log("Init");
      //Scena
      const scene = new THREE.Scene();
      //Camera
      const camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );

      //Render
      const renderer = new THREE.WebGLRenderer({
        canvas: document.querySelector("#tris3DCanvas"),
        antialias: true,
      });
      renderer.setSize(window.innerWidth, window.innerHeight);
      //Render Resize the windows
      window.addEventListener("resize", function () {
        var width = window.innerWidth;
        var height = window.innerHeight;
        renderer.setSize(width, height);
        camera.aspect = width / height;
        camera.updateProjectionMatrix();
      });
      //Controls
      const controls = new OrbitControls(camera, renderer.domElement);
      controls.enableZoom = true;

      // Lights
      const pointLight1 = new THREE.PointLight(0xffffff);
      const lightHelper1 = new THREE.PointLightHelper(pointLight1);
      pointLight1.position.set(100, 50, 100);
      pointLight1.intensity = 1;
      scene.add(pointLight1);

      //Helpers
      const gridHelper = new THREE.GridHelper(200, 50);
      scene.add(lightHelper1, gridHelper);

      const geometry = new THREE.BoxGeometry();
      const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
      const cube = new THREE.Mesh(geometry, material);
      scene.add(cube);

      camera.position.z = 5;

      function animate() {
        requestAnimationFrame(animate);

        cube.rotation.x += 0.01;
        cube.rotation.y += 0.01;

        renderer.render(scene, camera);
        controls.update();
      }
      animate();
    },
  },
  components: {},
  computed: {},
};
</script>