<template>
  <div></div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import Stats from "three/examples/jsm/libs/stats.module";
import { Interaction } from "three.interaction/src/index.js";

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
      var objIntersected = [];

      //Pointer for mouse movement
      const pointer = new THREE.Vector2();
      const radius = 100;

      init(); //Start
      animate(); //Refresh

      function init() {
        //Camera
        /* camera = new THREE.PerspectiveCamera(
          70,
          window.innerWidth / window.innerHeight,
          1,
          10000
        );*/
        camera = new THREE.PerspectiveCamera(
          30,
          window.innerWidth / window.innerHeight,
          0.1,
          5000
        );

        //Scene
        scene = new THREE.Scene();
        scene.background = new THREE.Color(0x000000);

        //Light
        const light = new THREE.DirectionalLight(0xffffff, 1);
        light.position.set(1, 1, 1).normalize();
        scene.add(light);

        //Helper
        const gridHelper = new THREE.GridHelper(200, 50);
        //scene.add(gridHelper);

        //Plane
        const geometryCube = new THREE.BoxGeometry(1, 0.1, 1);

        const materialOne = new THREE.MeshLambertMaterial({
          color: 0xffffff,
        });
        const materialTwo = new THREE.MeshLambertMaterial({
          color: 0xff0ff4c6,
        });
        let k = 0;
        for (let i = 0; i < 3; i++) {
          for (let j = 0; j < 3; j++) {
            const materialRandom = new THREE.MeshLambertMaterial({
              color: Math.random() * 0xffffff,
            });
            const object = new THREE.Mesh(geometryCube, materialRandom);
            k += 1;
            object.position.x = j;
            object.position.y = 0;
            object.position.z = i;
            object.cursor = "pointer";
            object.name = k;
            object.on("click", function (ev) {
              console.log(ev);
              console.log("Click obg " + ev.data.target.name);
              position(ev.data.target.name);
            });

            scene.add(object);
            objIntersected.push(object);
          }
        }

        /* const cube1 = new THREE.Mesh(geometryCube, materialTwo);
        cube1.position.x = -1;
        scene.add(cube1);
        objIntersected.push(cube1);
         cube1.cursor = "pointer";
        cube1.on("click", function (ev) {
          console.log(objIntersected);
          console.log("Click 1");
        });*/
        
        //Camera Position
        camera.position.z = 0;
        camera.position.x = 0;
        camera.position.y = 10;

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
        // controls.enableDamping = true; // an animation loop is required when either damping or auto-rotation are enabled
        // controls.dampingFactor = 0.05;

        //Stats for info
        stats = new Stats();
        container.appendChild(stats.dom);

        //Interaction
        const interaction = new Interaction(renderer, scene, camera);

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

      //Method for place X or O
      function position(k) {
        const geometryCube = new THREE.CylinderGeometry(0.5, 1, 2, 32);
        const materialRandom = new THREE.MeshLambertMaterial({
          color: Math.random() * 0xffffff,
        });
        const object = new THREE.Mesh(geometryCube, materialRandom);
        object.scale.x = 0.1;
        object.scale.z = 0.1;
        object.scale.y = 0.1;
        console.log("k " + k);
        var position = findPosition(k);
        console.log("Position " + position);
        object.position.y = 0.2;
        object.position.x = position[0];
        object.position.z = position[1];
        scene.add(object);
      }

      //Method Usufull for game logic
      function findPosition(k) {
        switch (k) {
          case 1:
            return [0, 0];
          case 2:
            return [1, 0];
          case 3:
            return [2, 0];
          case 4:
            return [0, 1];
          case 5:
            return [1, 1];
          case 6:
            return [2, 1];
          case 7:
            return [0, 2];
          case 8:
            return [1, 2];
          case 9:
            return [2, 2];
        }
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
        //camera.position.x = radius * Math.sin(THREE.MathUtils.degToRad(theta));
        //  camera.position.y = radius * Math.sin(THREE.MathUtils.degToRad(theta));
        //camera.position.z = radius * Math.cos(THREE.MathUtils.degToRad(theta));
        //camera.lookAt(scene.position);

        camera.updateMatrixWorld();

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
      //Other Methods
    },
  },
  components: {},
  computed: {},
};
</script>