<template>
  <div></div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import Stats from "three/examples/jsm/libs/stats.module";
import { Interaction } from "three.interaction/src/index.js";
import { RoundedBoxGeometry } from "three/examples/jsm/geometries/RoundedBoxGeometry.js";

export default {
  name: "tris3D",
  data() {
    return {
      data_tris: [
        [1, 1, 1],
        [1, 1, 1],
        [1, 1, 1],
      ],
      map_tris: {
        1: "00",
        2: "01",
        3: "02",
        4: "10",
        5: "11",
        6: "12",
        7: "20",
        8: "21",
        9: "22",
      },
    };
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

      //var logic game
      let turn = true;

      //Pointer for mouse movement
      const pointer = new THREE.Vector2();
      const radius = 100;
      var data_tris = this.data_tris;
      var map_tris = this.map_tris;
      init(); //Start
      animate(); //Refresh

      function init() {
        //Camera
        /*  camera = new THREE.PerspectiveCamera(
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
        light.position.set(1, 5, 1).normalize();
        const lightHelper1 = new THREE.PointLightHelper(light);
        scene.add(light);

        const pointLight2 = new THREE.PointLight(0xffffff);
        const lightHelper2 = new THREE.PointLightHelper(pointLight2);
        pointLight2.position.set(-1, -10, 1).normalize();
        pointLight2.intensity = 0.3;

        const ambientLight = new THREE.AmbientLight(0xffffff);
        ambientLight.intensity = 0.5;
        const ambientHelper = new THREE.PointLightHelper(ambientLight);

        scene.add(pointLight2, ambientLight);
        //Helper
        const gridHelper = new THREE.GridHelper(200, 50);
        // scene.add(lightHelper1, lightHelper2);
        //  scene.add(gridHelper);

        //Plane Separe
        //const separe = new THREE.BoxGeometry(3, 0.1, 0.05);
        const separe = new RoundedBoxGeometry(3, 0.15, 0.05, 8, 1);
        const materialSepare = new THREE.MeshLambertMaterial({
          color: 0x0000ff,
        });
        const altSepare = 0.07;
        const separe1 = new THREE.Mesh(separe, materialSepare);
        separe1.position.y = altSepare;
        separe1.position.z = 0.5;
        const separe2 = new THREE.Mesh(separe, materialSepare);
        separe2.position.y = altSepare;
        separe2.position.z = -0.5;
        const separe3 = new THREE.Mesh(separe, materialSepare);
        separe3.position.y = altSepare;
        separe3.position.x = 0.5;
        separe3.rotation.y = Math.PI / 2;
        const separe4 = new THREE.Mesh(separe, materialSepare);
        separe4.position.y = altSepare;
        separe4.position.x = -0.5;
        separe4.rotation.y = Math.PI / 2;
        scene.add(separe1, separe2, separe3, separe4);
        //Plane
        const geometryCube = new THREE.BoxGeometry(1, 0.1, 1);
        const materialOne = new THREE.MeshLambertMaterial({
          color: 0xffffff,
        });
        const materialTwo = new THREE.MeshLambertMaterial({
          color: 0xff0ff4c6,
        });
        let k = 0;
        for (let i = -1; i < 2; i++) {
          for (let j = -1; j < 2; j++) {
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
              console.log("Click obg " + ev.data.target.name);
              placeObj(ev.data.target.name);
            });

            scene.add(object);
            objIntersected.push(object);
          }
        }

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
      function placeObj(k) {
        var position = findPosition(k);
        //Control is empty 
        if (data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] == 1){
          if (turn) {
            var obg = createX();
            data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] = 3;
          } else {
            var obg = createO();
            data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] = 5;
          }
        turn = !turn; //change turn
        //Position obg
        obg.position.y = 0.2;
        obg.position.x = position[0];
        obg.position.z = position[1];

        scene.add(obg);
        }
      }

      //Object X
      function createX() {
        var altX = 0;
        const geometryX = new THREE.BoxGeometry(0.7, 0.15, 0.08);
        const materialX = new THREE.MeshLambertMaterial({ color: 0xffffff });
        const obj_X_one = new THREE.Mesh(geometryX, materialX);
        obj_X_one.position.y = altX;
        obj_X_one.position.x = 0;
        obj_X_one.rotation.y = Math.PI / 2 / 2;
        const obj_X_two = new THREE.Mesh(geometryX, materialX);
        obj_X_two.position.y = altX;
        obj_X_two.position.x = 0;
        obj_X_two.rotation.y = -(Math.PI / 2) / 2;
        const groupX = new THREE.Group();
        groupX.add(obj_X_one);
        groupX.add(obj_X_two);
        return groupX;
      }
      //Object O
      function createO() {
        var altO = 1;
        const materialO = new THREE.MeshLambertMaterial({ color: 0xffffff });
        var outerRadius = 0.3;
        var innerRadius = 0.2;
        var height = 0.05;

        var arcShape = new THREE.Shape();
        arcShape.moveTo(outerRadius * 2, outerRadius);
        arcShape.absarc(
          outerRadius,
          outerRadius,
          outerRadius,
          0,
          Math.PI * 2,
          false
        );
        var holePath = new THREE.Path();
        holePath.moveTo(outerRadius + innerRadius, outerRadius);
        holePath.absarc(
          outerRadius,
          outerRadius,
          innerRadius,
          0,
          Math.PI * 2,
          true
        );
        arcShape.holes.push(holePath);

        var geometryO = new THREE.ExtrudeGeometry(arcShape, {
          amount: height,
          bevelEnabled: false,
          steps: 1,
          curveSegments: 60,
        });
        geometryO.center();
        geometryO.rotateX(Math.PI * -0.5);
        var objO = new THREE.Mesh(geometryO, materialO);
        objO.position.y = altO;

        return objO;
      }
      //Method Usufull for game logic
      function findPosition(k) {
        switch (k) {
          case 1:
            return [-1, -1];
          case 2:
            return [0, -1];
          case 3:
            return [1, -1];
          case 4:
            return [-1, 0];
          case 5:
            return [0, 0];
          case 6:
            return [1, 0];
          case 7:
            return [-1, 1];
          case 8:
            return [0, 1];
          case 9:
            return [1, 1];
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