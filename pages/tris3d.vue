<template>
  <div></div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import Stats from "three/examples/jsm/libs/stats.module";

export default {
  name: "tris3D",
  data() {
    return {};
  },
  mounted() {
    this.init2();
    console.log("Mounted");
  },
  methods: {
    /*  init() {
      console.log("Init");
      //Utils
      const pointer = new THREE.Vector2();
      let container, stats;
      let raycaster;

      let INTERSECTED;
      let theta = 0;
      stats = new Stats();
      raycaster = new THREE.Raycaster();
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

      //Plane
      const planeGroup = new THREE.Group();
      const geometryCube = new THREE.BoxGeometry(1, 0.1, 1);
      const materialOne = new THREE.MeshBasicMaterial({ color: 0xffffed00 });
      const materialTwo = new THREE.MeshBasicMaterial({ color: 0xff0ff4c6 });
      const cube1 = new THREE.Mesh(geometryCube, materialOne);
      const cube2 = new THREE.Mesh(geometryCube, materialTwo);
      cube2.position.x = -1;
      //planeGroup.add(cube1, cube2);
      scene.add(cube1, cube2);

      camera.position.z = 5;

      document.addEventListener("mousemove", onPointerMove);
      function onPointerMove(event) {
        pointer.x = (event.clientX / window.innerWidth) * 2 - 1;
        pointer.y = -(event.clientY / window.innerHeight) * 2 + 1;
      }

      function animate() {
        //Request
        requestAnimationFrame(animate);

        //Intersects
        raycaster.setFromCamera(pointer, camera);

        const intersects = raycaster.intersectObjects(scene.children, false);
        if (intersects.length > 0) {
          if (INTERSECTED != intersects[0].object) {
            if (INTERSECTED)
              INTERSECTED.material.emissive.setHex(INTERSECTED.currentHex);

            INTERSECTED = intersects[0].object;
            INTERSECTED.currentHex = INTERSECTED.material.emissive.getHex();
            INTERSECTED.material.emissive.setHex(0xff0000);
          }
        } else {
          if (INTERSECTED)
            INTERSECTED.material.emissive.setHex(INTERSECTED.currentHex);

          INTERSECTED = null;
        }

        //Update
        renderer.render(scene, camera);
        controls.update();
        stats.update();
      }
      animate();
    },*/

    init2() {
      //Var Useful
      let container, stats, controls;
      let camera, scene, raycaster, renderer;

      let INTERSECTED;
      let theta = 0;

      var objIntersected = [];

      //Pointer for mouse movement
      const pointer = new THREE.Vector2();
      const radius = 100;

      init(); //Start
      animate(); //Refresh

      function init() {
        //Camera
        camera = new THREE.PerspectiveCamera(
          70,
          window.innerWidth / window.innerHeight,
          1,
          10000
        );

        //Scene
        scene = new THREE.Scene();
        scene.background = new THREE.Color(0xf0f0f0);

        //Light
        const light = new THREE.DirectionalLight(0xffffff, 1);
        light.position.set(1, 1, 1).normalize();
        scene.add(light);

        //Helper
        const gridHelper = new THREE.GridHelper(200, 50);
        scene.add(gridHelper);

        //Plane
        /*const geometry = new THREE.BoxGeometry(20, 20, 20);

         const object = new THREE.Mesh(
          geometry,
          new THREE.MeshLambertMaterial({ color: Math.random() * 0xffffff })
        );
        scene.add(object);
        objIntersected.push(object);*/
        const geometryCube = new THREE.BoxGeometry(1, 0.1, 1);
        const materialOne = new THREE.MeshLambertMaterial({ color: 0xffffed00 });
        const materialTwo = new THREE.MeshLambertMaterial({ color: 0xff0ff4c6 });
        const cube1 = new THREE.Mesh(geometryCube, materialOne);
        const cube2 = new THREE.Mesh(geometryCube, materialTwo);
        cube2.position.x = -1;
        scene.add(cube1, cube2);
        objIntersected.push(cube1);
        objIntersected.push(cube2);
        camera.position.z = 0;
        camera.position.x = 0;
        camera.position.y = 0;
        
        //RayCaster declaration
        raycaster = new THREE.Raycaster();

        //Render
        container = document.createElement("div");
        document.body.appendChild(container); //Append 3d in to html
        renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        container.appendChild(renderer.domElement);

        //Controls
        const controls = new OrbitControls(camera, renderer.domElement);
        controls.enableZoom = true;
        controls.enableDamping = true; // an animation loop is required when either damping or auto-rotation are enabled
        controls.dampingFactor = 0.05;

        //Stats for info
        stats = new Stats();
        container.appendChild(stats.dom);

        //Listener for mouse movement
        document.addEventListener("mousemove", onPointerMove);

        //Listener Resize Page
        window.addEventListener("resize", onWindowResize);
      }

      //Resize Page
      function onWindowResize() {
        camera.aspect = window.innerWidth / window.innerHeight;
        camera.updateProjectionMatrix();
        renderer.setSize(window.innerWidth, window.innerHeight);
      }

      //Event MoveMouse / Pointer
      function onPointerMove(event) {
        pointer.x = (event.clientX / window.innerWidth) * 2 - 1;
        pointer.y = -(event.clientY / window.innerHeight) * 2 + 1;
      }

      //Animate Loop
      function animate() {
        requestAnimationFrame(animate);

        //controls.update();
        stats.update();
        render();
      }

      //Render Loop
      function render() {
        //theta += 0.1;

        //camera.position.x = radius * Math.sin(THREE.MathUtils.degToRad(theta));
        //  camera.position.y = radius * Math.sin(THREE.MathUtils.degToRad(theta));
        //camera.position.z = radius * Math.cos(THREE.MathUtils.degToRad(theta));
        //camera.lookAt(scene.position);

        //   camera.updateMatrixWorld();

        // Find intersections
        raycaster.setFromCamera(pointer, camera);

        //const intersects = raycaster.intersectObjects(scene.children, false);
        const intersects = raycaster.intersectObjects(objIntersected, false);

        if (intersects.length > 0) {
          if (INTERSECTED != intersects[0].object) {
            if (INTERSECTED)
              INTERSECTED.material.emissive.setHex(INTERSECTED.currentHex);

            INTERSECTED = intersects[0].object;
            INTERSECTED.currentHex = INTERSECTED.material.emissive.getHex();
            INTERSECTED.material.emissive.setHex(0xff0000);
          }
        } else {
          if (INTERSECTED)
            INTERSECTED.material.emissive.setHex(INTERSECTED.currentHex);

          INTERSECTED = null;
        }

        //ReRender
        renderer.render(scene, camera);
       // controls.update();
      }
    },
  },
  components: {},
  computed: {},
};
</script>