<template>
  <div>
    <!-- Container Menu Game Center -->
    <div
      class="absolute text-center mx-auto grid grid-cols-2 gap-4"
      style="
        color: white;
        left: 50%;
        top: 80%;
        transform: translate(-50%, -50%);
      "
      v-show="data.showGameMenu"
    >
      <!-- Button Play -->
      <button
        class="
          col-span-2
          text-center
          bg-green-500
          hover:bg-green-700
          text-white
          font-bold
          py-2
          px-4
          rounded-2xl
          text-2xl
          md:text-3xl
          lg:text-5xl
        "
        @click="data.isGameStart = true"
      >
        PLAY
      </button>
      <!-- Game Setting -->
      <!-- Robot vs Player -->
      <div
        @click="data.robot = !data.robot"
        class="
          hover:bg-blue-700
          text-white
          font-bold
          py-2
          px-4
          rounded-full
          cursor-pointer
          text-lg
          md:text-xl
          lg:text-3xl
        "
        v-bind:class="[!data.robot ? 'bg-gray-500' : 'bg-blue-500']"
        style=""
      >
        Player vs Robot
      </div>
      <!--Player vs Player  -->
      <div
        @click="data.robot = !data.robot"
        class="
          hover:bg-blue-700
          text-white
          font-bold
          py-2
          px-4
          rounded-full
          cursor-pointer
          text-lg
          md:text-xl
          lg:text-3xl
        "
        v-bind:class="[data.robot ? 'bg-gray-500' : 'bg-blue-500']"
        style=""
      >
        Player vs Player
      </div>
    </div>
    <!-- Container Menu Pause -->
    <div
      class="
        absolute
        text-center
        mx-auto
        grid grid-cols-2
        gap-4
        text-xl
        md:text-3xl
        lg:text-5xl
      "
      style="
        color: white;
        left: 50%;
        top: 80%;
        transform: translate(-50%, -50%);
      "
      v-show="data.showPause"
    >
      <!-- Button Restart -->
      <button
        class="
          col-span-2
          text-center
          bg-green-500
          hover:bg-green-700
          text-white
          font-bold
          py-2
          px-4
          rounded-2xl
          text-2xl
          md:text-3xl
          lg:text-5xl
        "
        @click="data.btnResetPlay = true"
      >
        Restart
      </button>
      <!-- Button Menu -->
      <button
        class="
          col-span-2
          text-center
          bg-blue-500
          hover:bg-blue-700
          text-white
          font-bold
          py-2
          px-4
          rounded-2xl
          text-2xl
          md:text-3xl
          lg:text-5xl
        "
        @click="data.btnshowMenu = true"
      >
        Menu
      </button>
    </div>
    <!-- Show Turn Player -->
    <div
      class="
        absolute
        text-center
        mx-auto
        font-bold
        py-2
        px-4
        rounded-full
        text-2xl
        md:text-3xl
        lg:text-5xl
      "
      style="left: 50%; top: 0; transform: translate(-50%, 0)"
      v-show="data.isGameStart"
    >
      <h1 v-if="data.turn" style="color: yellow">Turn X</h1>
      <h1 v-if="!data.turn" style="color: rgb(255, 48, 127)">Turn O</h1>
    </div>
    <!-- Camera Rotation On/Off -->
    <svg
      @click="data.isRotateCamera = !data.isRotateCamera"
      class="
        absolute
        opacity-50
        hover:opacity-100
        text-white
        font-bold
        py-2
        px-4
        rounded-full
        cursor-pointer
      "
      style="left: 3px; bottom: 3px; color: white; fill: white"
      version="1.1"
      id="Layer_1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      x="0px"
      y="0px"
      width="81px"
      height="62px"
      viewBox="0 0 122.879 93.242"
      enable-background="new 0 0 122.879 93.242"
      xml:space="preserve"
    >
      <g>
        <path
          fill-rule="evenodd"
          clip-rule="evenodd"
          d="M51.933,0.036h31.521l7.185,12.79h14.794c0.786,0,1.428,0.662,1.428,1.429v35.003 c5.257,1.931,9.185,4.161,11.755,6.539c2.808,2.599,4.234,5.514,4.264,8.602c0.03,3.092-1.35,6.024-4.156,8.649 c-4.786,4.479-14.429,8.444-28.976,10.897c-1.671,0.277-3.223-1.036-3.467-2.935s0.911-3.662,2.582-3.94 c13.403-2.26,22.011-5.655,25.986-9.375c1.301-1.216,1.942-2.312,1.934-3.242c-0.009-0.936-0.684-2.043-2.017-3.276 c-1.765-1.633-4.401-3.2-7.905-4.633v7.618c0,0.766-0.66,1.428-1.428,1.428H19.506c-0.767,0-1.429-0.643-1.429-1.428v-8.5 c-5.576,1.91-9.059,4.052-10.86,6.261c-1.003,1.23-1.261,2.413-0.937,3.497c0.472,1.573,1.877,3.242,3.958,4.875 c7.076,5.552,20.555,9.51,33.239,8.611l-2.744-2.146c-1.311-1.023-1.544-2.917-0.52-4.228c1.024-1.312,2.917-1.544,4.229-0.521 l8.878,6.942c1.311,1.024,1.544,2.917,0.52,4.229c-0.132,0.17-0.279,0.32-0.438,0.454l-9.882,8.837 c-1.239,1.109-3.143,1.003-4.252-0.236c-1.108-1.238-1.003-3.143,0.236-4.251l2.28-2.039C28.4,86.376,14.414,81.996,6.771,76 c-3.146-2.468-5.364-5.305-6.277-8.353c-1.06-3.537-0.466-7.092,2.251-10.425c2.599-3.188,7.505-6.194,15.332-8.696V14.255 c0-0.786,0.642-1.429,1.429-1.429h6.652v-4.59h8.22v4.59h8.928c1.858-3.667,3.715-7.335,5.574-11 C50.01-0.411,49.389,0.036,51.933,0.036L51.933,0.036z M97.607,19.144c2.353,0,4.261,1.909,4.261,4.262s-1.908,4.262-4.261,4.262 s-4.262-1.909-4.262-4.262S95.255,19.144,97.607,19.144L97.607,19.144L97.607,19.144z M66.229,24.113 c7.134,0,12.919,5.785,12.919,12.918s-5.785,12.917-12.919,12.917c-7.135,0-12.92-5.785-12.92-12.917 C53.31,29.898,59.094,24.113,66.229,24.113L66.229,24.113z M66.229,15.696c11.782,0,21.336,9.555,21.336,21.335 s-9.554,21.336-21.336,21.336c-11.781,0-21.335-9.556-21.335-21.336S54.448,15.696,66.229,15.696L66.229,15.696z"
        />
      </g>
    </svg>
  </div>
</template>

<script>
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import Stats from "three/examples/jsm/libs/stats.module";
import { Interaction } from "three.interaction/src/index.js";
import { RoundedBoxGeometry } from "three/examples/jsm/geometries/RoundedBoxGeometry.js";
import { FontLoader } from "three/examples/jsm/loaders/FontLoader.js";
import { TextGeometry } from "three/examples/jsm/geometries/TextGeometry.js";

export default {
  name: "tris3D",
  data() {
    return {
      data: {
        player: false,
        robot: true,
        isFull: false,
        gameWinner: "",
        isRotateCamera: true,
        cameraControll: true,
        turn: true,
        isGameEnd: false,
        isGameStart: false,

        showGameMenu: true,
        showPause: false,
        btnshowMenu: false,
        btnResetPlay: false,
        reset: false,
      },
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
      gameHistory: {
        victory_x: 0,
        victory_o: 0,
        victory_parity: 0,
      },
    };
  },
  mounted() {
    this.render();
    console.log("Mounted");
  },
  methods: {
    render() {
      THREE.Cache.enabled = true;
      //Var Useful
      let container, stats, controls;
      let camera, scene, raycaster, renderer;

      //var animation
      var mixer;
      var clock = new THREE.Clock();
      var anim_x_o = [];

      //Var for text
      var textMenu;
      var textMenu3D;

      let INTERSECTED;
      var objIntersected = [];

      //Pointer for mouse movement
      const pointer = new THREE.Vector2();
      const radius = 100;

      //Varibili globali
      var data_tris = this.data_tris;
      var map_tris = this.map_tris;
      var data = this.data;

      //var color
      var color_bg = new THREE.Color("rgb(0, 0, 0)");
      var color_plane = new THREE.Color("rgb(2, 52, 54)");
      var color_separe = new THREE.Color("rgb(17, 17, 17)");
      var color_borderPlane = new THREE.Color("rgb(0, 34, 35)");
      var color_x = new THREE.Color("rgb(254, 198, 1)");
      var color_o = new THREE.Color("rgb(183, 36, 92)");
      var color_text_front = new THREE.Color("rgb(255, 195, 0)");
      var color_text_side = new THREE.Color("rgb(81, 0, 135)");

      //Var Urls
      var urlFonts = "../font1.json";

      init(); //Start
      animate(); //Refresh

      function init() {
        //Camera
        camera = new THREE.PerspectiveCamera(
          30,
          window.innerWidth / window.innerHeight,
          0.1,
          5000
        );

        //Scene
        scene = new THREE.Scene();
        scene.background = color_bg;

        //Light
        const light = new THREE.DirectionalLight(0xffffff, 1.5);
        light.position.set(1, 5, 1);
        light.castShadow = true;
        //.normalize();
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

        //BG Object Random
        for (let i = 0; i < 200; i++) {
          if (i % 2 == 0) {
            var object = createO(1);
          } else {
            var object = createX(1);
          }

          do {
            var random_x = generateRandom(-10, 10);
            var random_z = generateRandom(-10, 10);
            var abs = Math.abs(random_x) + Math.abs(random_z);
          } while (abs == 2 || abs == 0 || abs == 1 || abs == 3);
          object.position.x = random_x;
          object.position.z = random_z;
          object.position.y = generateRandom(-10, 10);

          object.rotation.x = Math.random() * 2 * Math.PI;
          object.rotation.y = Math.random() * 2 * Math.PI;
          object.rotation.z = Math.random() * 2 * Math.PI;

          const scaleRandom = Math.random() + 0.5;
          object.scale.x = scaleRandom;
          object.scale.y = scaleRandom;
          object.scale.z = scaleRandom;

          scene.add(object);
        }

        //Plane Separe
        const separe = new RoundedBoxGeometry(3.2, 0.15, 0.05, 8, 1);
        const materialSepare = new THREE.MeshLambertMaterial({
          color: color_separe,
        });
        const altSepare = 0.07;
        const separe1 = new THREE.Mesh(separe, materialSepare);
        separe1.position.y = altSepare;
        separe1.position.z = 0.5;
        separe1.receiveShadow = true;
        const separe2 = new THREE.Mesh(separe, materialSepare);
        separe2.position.y = altSepare;
        separe2.position.z = -0.5;
        separe2.receiveShadow = true;
        const separe3 = new THREE.Mesh(separe, materialSepare);
        separe3.position.y = altSepare;
        separe3.position.x = 0.5;
        separe3.rotation.y = Math.PI / 2;
        separe3.receiveShadow = true;
        const separe4 = new THREE.Mesh(separe, materialSepare);
        separe4.position.y = altSepare;
        separe4.position.x = -0.5;
        separe4.rotation.y = Math.PI / 2;
        separe4.receiveShadow = true;
        scene.add(separe1, separe2, separe3, separe4);

        //Border Plane
        const geometry_borderPlane = new THREE.BoxGeometry(3.2, 0.1, 0.1);
        const materialBorderPlane = new THREE.MeshLambertMaterial({
          color: color_borderPlane,
        });
        const borderPlane1 = new THREE.Mesh(
          geometry_borderPlane,
          materialBorderPlane
        );
        borderPlane1.position.z = 1.55;
        borderPlane1.receiveShadow = true;
        const borderPlane2 = new THREE.Mesh(
          geometry_borderPlane,
          materialBorderPlane
        );
        borderPlane2.position.z = -1.55;
        borderPlane2.receiveShadow = true;
        const borderPlane3 = new THREE.Mesh(
          geometry_borderPlane,
          materialBorderPlane
        );
        borderPlane3.position.x = -1.55;
        borderPlane3.receiveShadow = true;
        borderPlane3.rotation.y = Math.PI / 2;
        const borderPlane4 = new THREE.Mesh(
          geometry_borderPlane,
          materialBorderPlane
        );
        borderPlane4.position.x = 1.55;
        borderPlane4.receiveShadow = true;
        borderPlane4.rotation.y = Math.PI / 2;
        scene.add(borderPlane1, borderPlane2, borderPlane3, borderPlane4);
        //Plane
        const geometryCube = new THREE.BoxGeometry(1, 0.1, 1);
        const materialOne = new THREE.MeshLambertMaterial({
          color: 0xffffff,
        });
        const materialTwo = new THREE.MeshLambertMaterial({
          color: 0xff0ff4c6,
        });
        //Generate Game Plane
        let k = 0;
        for (let i = -1; i < 2; i++) {
          for (let j = -1; j < 2; j++) {
            const materialRandom = new THREE.MeshLambertMaterial({
              // color: Math.random() * 0xffffff,
              color: color_plane,
            });
            const object = new THREE.Mesh(geometryCube, materialRandom);
            k += 1;
            object.position.x = j;
            object.position.y = 0;
            object.position.z = i;
            object.cursor = "pointer";
            object.name = k;
            //Click on plane
            object.on("click", function (ev) {
              if (data.isGameStart) {
                console.log("Click " + ev.data.target.name);
                //Controllo per vedere se si clicca su un obg gia messo
                if (
                  data_tris[map_tris[ev.data.target.name].charAt(0)][
                    map_tris[ev.data.target.name].charAt(1)
                  ] == 1 &&
                  !data.isGameEnd &&
                  !data.isFull
                ) {
                  placeObj(ev.data.target.name);
                  if (data.robot && !data.isGameEnd)
                    // setTimeout(() => {
                    playRobot();
                  //}, 500);
                }
              }
            });

            object.receiveShadow = true;
            scene.add(object);
            objIntersected.push(object);
          }
        }

        //Camera Position
        camera.position.z = 5;
        camera.position.x = 4;
        camera.position.y = 5;

        //RayCaster declaration
        raycaster = new THREE.Raycaster();

        //Render
        container = document.createElement("div");
        document.body.appendChild(container); //Append 3d in to html
        renderer = new THREE.WebGLRenderer({ antialias: true });
        renderer.shadowMap.enabled = true;
        renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        renderer.setPixelRatio(window.devicePixelRatio);
        renderer.setSize(window.innerWidth, window.innerHeight);
        container.appendChild(renderer.domElement);

        //Controls
        controls = new OrbitControls(camera, renderer.domElement);
        controls.enabled = data.cameraControll;
        controls.enableZoom = true;
        controls.autoRotate = data.isRotateCamera;
        controls.autoRotateSpeed = 1;
        controls.enablePan = false;
        controls.maxDistance = 50;
        controls.minDistance = 2;
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

        //Create Lobby
        lobby();
      } // Init end

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
        if (data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] == 1) {
          if (data.turn) {
            var obg = createX();
            (obg.name = "x"),
              (data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] = 5);
          } else {
            var obg = createO();
            (obg.name = "o"),
              (data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] = 3);
          }
          data.turn = !data.turn; //change turn
          //Position obg
          //obg.position.y = 0.1;
          obg.position.y = 0.3;
          obg.position.x = position[0];
          obg.position.z = position[1];
          anim_x_o.push(obg);
          scene.add(obg);
        }
        // this.controll(); //controll winner

        //Controll is Full
        var find = false;
        for (var i = 0; i < 3 && !find; i++) {
          for (var j = 0; j < 3 && !find; j++) {
            if (data_tris[i][j] == 1) {
              find = true;
            }
          }
        }
        if (find) data.isFull = false;
        else data.isFull = true;

        controll();
      }

      //Object X
      function createX(x) {
        var altX = 0;
        const geometryX = new THREE.BoxGeometry(0.7, 0.15, 0.08);
        if (x != null)
          var materialX = new THREE.MeshLambertMaterial({
            color: Math.random() * 0xffffff,
          });
        else var materialX = new THREE.MeshPhongMaterial({ color: color_x });
        const obj_X_one = new THREE.Mesh(geometryX, materialX);
        obj_X_one.position.y = altX;
        obj_X_one.position.x = 0;
        obj_X_one.rotation.y = Math.PI / 2 / 2;
        obj_X_one.castShadow = true;
        obj_X_one.receiveShadow = true;
        const obj_X_two = new THREE.Mesh(geometryX, materialX);
        obj_X_two.position.y = altX;
        obj_X_two.position.x = 0;
        obj_X_two.rotation.y = -(Math.PI / 2) / 2;
        obj_X_two.castShadow = true;
        obj_X_two.receiveShadow = true;
        const groupX = new THREE.Group();
        groupX.add(obj_X_one);
        groupX.add(obj_X_two);
        return groupX;
      }
      //Object O
      function createO(x) {
        var altO = 1;
        if (x != null)
          var materialO = new THREE.MeshLambertMaterial({
            color: Math.random() * 0xffffff,
          });
        else var materialO = new THREE.MeshPhongMaterial({ color: color_o });

        var outerRadius = 0.3;
        var innerRadius = 0.2;
        var height = 0.15;

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
        objO.castShadow = true;
        objO.receiveShadow = true;
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
        //Control Camera
        controls.autoRotate = data.isRotateCamera;
        controls.update();
        stats.update();
        render();
      }

      //Render Loop
      function render() {
        camera.updateMatrixWorld();

        //StartGame
        if (data.isGameStart) {
          scene.remove(scene.getObjectByName("textMenuTRIS"));
          scene.remove(scene.getObjectByName("textMenu3D"));
          data.showGameMenu = false;
        }
        //Reset Game
        if (data.btnResetPlay) {
          restartGame();
          data.btnResetPlay = false;
        }
        //Go menu
        if (data.btnshowMenu) {
          goMenu();
          data.btnshowMenu = false;
        }

        //Animation Text Game End
        if (data.isGameEnd) {
          //  var obj = scene.getObjectByName("textVictory");
          //  if (obj.rotation.x < 5.8) obj.rotation.x += 0.07;
        }

        //Animation
        if (scene.getObjectByName("textMenu3D") != null) rotateCube();
        //X and O
        Func_anim_x_o();
        // var delta = clock.getDelta();
        // if (mixer) mixer.update(delta);

        // Find intersections
        raycaster.setFromCamera(pointer, camera);

        //const intersects = raycaster.intersectObjects(scene.children, false);
        const intersects = raycaster.intersectObjects(objIntersected, false);

        if (intersects.length > 0 && data.isGameStart) {
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
      //Method for controll winner
      function controll() {
        var winner = "";
        //Controllo Righe
        for (var i = 0; i < 3; i++) {
          var addRig = 1;
          for (var j = 0; j < 3; j++) {
            addRig = data_tris[i][j] * addRig;
          }
          if (addRig == 27) {
            winner = "o";
          }
          if (addRig == 125) {
            winner = "x";
          }
        }
        //Controllo Colonne
        for (var i = 0; i < 3; i++) {
          var addCol = 1;
          for (var j = 0; j < 3; j++) {
            addCol = data_tris[j][i] * addCol;
          }
          if (addCol == 27) {
            winner = "o";
          }
          if (addCol == 125) {
            winner = "x";
          }
        }
        //Controllo Diagonale Sinistra
        var addDig1 = 1;
        for (var i = 0; i < 3; i++) {
          addDig1 = data_tris[i][i] * addDig1;
          if (addDig1 == 27) {
            winner = "o";
          }
          if (addDig1 == 125) {
            winner = "x";
          }
        }
        //Controllo Diagonale Destra
        var addDig2 = data_tris[0][2] * data_tris[1][1] * data_tris[2][0];
        if (addDig2 == 27) {
          winner = "o";
        }
        if (addDig2 == 125) {
          winner = "x";
        }
        //Final Controll
        if (winner == "x") data.gameWinner = "X Victory";
        if (winner == "o") data.gameWinner = "O Victory";
        if (data.gameWinner == "") {
          if (data.isFull) {
            data.gameWinner = "Parity";
            endGame();
          }
        } else {
          endGame();
        }
      }
      //Method for AI Robot
      function playRobot() {
        console.log("Gioca il robot");
        var random = true;
        var isValuePut = true;

        //MOSSE VINCENTI
        //Play Rows
        if (isValuePut) {
          for (var i = 0; i < 3; i++) {
            var calcRiga = 1;
            for (var j = 0; j < 3; j++) {
              calcRiga = data_tris[i][j] * calcRiga;
            }
            if (calcRiga == 9) {
              for (var j = 0; j < 3; j++) {
                if (data_tris[i][j] == 1) {
                  placeObj(getPositionTrisMap(i, j));
                  isValuePut = false;
                }
              }
            }
          }
        }

        //Play Columns
        if (isValuePut) {
          for (var i = 0; i < 3; i++) {
            var calcColum = 1;
            for (var j = 0; j < 3; j++) {
              calcColum = data_tris[j][i] * calcColum;
            }
            if (calcColum == 9) {
              for (var j = 0; j < 3; j++) {
                if (data_tris[j][i] == 1) {
                  placeObj(getPositionTrisMap(j, i));
                  isValuePut = false;
                }
              }
            }
          }
        }

        //Play Diagonal Sinistra
        if (isValuePut) {
          var calcDiagonalSx =
            data_tris[0][0] * data_tris[1][1] * data_tris[2][2];
          if (calcDiagonalSx == 9) {
            for (var j = 0; j < 3; j++) {
              if (data_tris[j][j] == 1) {
                placeObj(getPositionTrisMap(j, j));
                isValuePut = false;
              }
            }
          }
        }

        //Play Diagonal Destra
        if (isValuePut) {
          var calcDiagonalSx =
            data_tris[0][2] * data_tris[1][1] * data_tris[2][0];
          if (calcDiagonalSx == 9) {
            var i = 0;
            var j = 2;
            for (var k = 0; k < 3; k++) {
              if (data_tris[i][j] == 1) {
                placeObj(getPositionTrisMap(i, j));
                isValuePut = false;
              }
              i += 1;
              j -= 1;
            }
          }
        }

        //MOSSE BLOCANTI
        //Play Rows
        if (isValuePut) {
          for (var i = 0; i < 3; i++) {
            var calcRiga = 1;
            for (var j = 0; j < 3; j++) {
              calcRiga = data_tris[i][j] * calcRiga;
            }
            if (calcRiga == 25) {
              for (var j = 0; j < 3; j++) {
                if (data_tris[i][j] == 1) {
                  placeObj(getPositionTrisMap(i, j));
                  isValuePut = false;
                }
              }
            }
          }
        }
        //Play Columns
        if (isValuePut) {
          for (var i = 0; i < 3; i++) {
            var calcColum = 1;
            for (var j = 0; j < 3; j++) {
              calcColum = data_tris[j][i] * calcColum;
            }
            if (calcColum == 25) {
              for (var j = 0; j < 3; j++) {
                if (data_tris[j][i] == 1) {
                  placeObj(getPositionTrisMap(j, i));
                  isValuePut = false;
                }
              }
            }
          }
        }

        //Play Diagonal Sinistra
        if (isValuePut) {
          var calcDiagonalSx =
            data_tris[0][0] * data_tris[1][1] * data_tris[2][2];
          if (calcDiagonalSx == 25) {
            for (var j = 0; j < 3; j++) {
              if (data_tris[j][j] == 1) {
                placeObj(getPositionTrisMap(j, j));
                isValuePut = false;
              }
            }
          }
        }

        //Play Diagonal Destra
        if (isValuePut) {
          var calcDiagonalSx =
            data_tris[0][2] * data_tris[1][1] * data_tris[2][0];
          if (calcDiagonalSx == 25) {
            var i = 0;
            var j = 2;
            for (var k = 0; k < 3; k++) {
              if (data_tris[i][j] == 1) {
                placeObj(getPositionTrisMap(i, j));
                isValuePut = false;
              }
              i += 1;
              j -= 1;
            }
          }
        }

        //TEST IMPOSSIBLE Victory
        //Play Diagonal Destra
        if (isValuePut) {
          if (data_tris[1][1] == 1)
            if (
              data_tris[0][0] == 5 ||
              data_tris[0][2] == 5 ||
              data_tris[2][0] == 5 ||
              data_tris[2][2] == 5
            ) {
              placeObj(getPositionTrisMap(1, 1));
              isValuePut = false;
            }
        }

        //Position Random
        if (isValuePut && !data.isFull) {
          while (random) {
            var numRandom = Math.floor(Math.random() * 9) + 1;
            if (
              data_tris[map_tris[numRandom].charAt(0)][
                map_tris[numRandom].charAt(1)
              ] == 1
            ) {
              random = false;
              isValuePut = false;
              placeObj(numRandom);
            }
          }
        }
      }

      //Position Numeber da i, j
      function getPositionTrisMap(i, j) {
        switch (i) {
          case 0:
            switch (j) {
              case 0:
                return 1;
              case 1:
                return 2;
              case 2:
                return 3;
            }
          case 1:
            switch (j) {
              case 0:
                return 4;
              case 1:
                return 5;
              case 2:
                return 6;
            }
          case 2:
            switch (j) {
              case 0:
                return 7;
              case 1:
                return 8;
              case 2:
                return 9;
            }
        }
      }
      //View Winner
      function endGame() {
        //Set Game End
        data.isGameStart = false;
        data.isGameEnd = true;
        data.showPause = true;
        //TEXT VICTORY
        var loaderText = new FontLoader();
        loaderText.load(urlFonts, function (font) {
          const geometry = new TextGeometry(data.gameWinner, {
            font: font,
            size: 0.5,
            height: 0.3,
          });
          geometry.computeBoundingBox();
          geometry.center();
          const materials = [
            new THREE.MeshPhongMaterial({ color: color_text_front }), // front
            new THREE.MeshPhongMaterial({ color: color_text_side }), // side
          ];
          const textVictory = new THREE.Mesh(geometry, materials);
          textVictory.castShadow = true;
          textVictory.position.y = 1.5;
          textVictory.position.x = 0;
          textVictory.rotation.x = -Math.PI / 2 / 2;
          textVictory.name = "textVictory";
          scene.add(textVictory);

          //Reposition camera
          camera.position.z = 10;
          camera.position.x = 0;
          camera.position.y = 5;
          data.isRotateCamera = true;
          controls.autoRotate = data.isRotateCamera;
          controls.autoRotateSpeed = 0.3;
        });
      }
      //Random Value
      function generateRandom(min, max) {
        var num = Math.floor(Math.random() * (max - min + 1)) + min;
        return num;
        /* return num === -1 || num === 1 || num === 0
          ? generateRandom(min, max)
          : num;
          */
      }
      //Lobby
      function lobby() {
        //Text Menu
        var loaderText = new FontLoader();
        loaderText.load(urlFonts, function (font) {
          const geometry = new TextGeometry("TRIS", {
            font: font,
            size: 0.5,
            height: 0.3,
          });
          geometry.computeBoundingBox();
          geometry.center();
          const materials = [
            new THREE.MeshPhongMaterial({ color: color_text_front }), // front
            new THREE.MeshPhongMaterial({ color: color_text_side }), // side
          ];
          textMenu = new THREE.Mesh(geometry, materials);
          textMenu.castShadow = true;
          textMenu.position.y = 1.5;
          textMenu.position.x = -0.5;
          textMenu.rotation.x = -Math.PI / 2 / 2;
          textMenu.name = "textMenuTRIS";
          scene.add(textMenu);

          //mixer = new THREE.AnimationMixer(textMenu);
          //let animation = THREE.AnimationClip(100,"y");
          //let animation2 = new THREE.get({loop:true}).to({opacity:1},500).wait("1500").to({opacity:0},500)
          //mixer.clipAction(animation2).play();
          //console.log(mixer);
        });
        var loaderText = new FontLoader();

        loaderText.load(urlFonts, function (font) {
          const geometry = new TextGeometry("3D", {
            font: font,
            size: 0.5,
            height: 0.3,
          });
          geometry.computeBoundingBox();
          geometry.center();
          const materials = [
            new THREE.MeshPhongMaterial({ color: color_text_front }), // front
            new THREE.MeshPhongMaterial({ color: color_text_side }), // side
          ];
          textMenu3D = new THREE.Mesh(geometry, materials);
          textMenu3D.castShadow = true;
          textMenu3D.position.y = 1.5;
          textMenu3D.position.x = 0.9;
          textMenu3D.rotation.x = -Math.PI / 2 / 2;
          textMenu3D.name = "textMenu3D";
          scene.add(textMenu3D);
        });
        data.turn = Boolean(Math.round(Math.random()));
      }

      function rotateCube() {
        var SPEED = 0.01;
        //if (textMenu3D.rotation.y > -0.9) textMenu3D.rotation.y -= SPEED;
        // else
        textMenu3D.rotation.x += SPEED;
        //textMenu3D.rotation.y -= SPEED;
        //textMenu3D.rotation.z -= SPEED *0.2;
      }
      //Restart New Game
      function restartGame() {
        //Clear Obg on plane
        for (var i = 0; i < 9; i++) {
          scene.remove(scene.getObjectByName("x"));
          scene.remove(scene.getObjectByName("o"));
        }
        scene.remove(scene.getObjectByName("textVictory"));

        //Reset Impostazioni
        for (var i = 0; i < 3; i++) {
          for (var j = 0; j < 3; j++) {
            data_tris[i][j] = 1;
          }
        }

        data.player = false;
        //data.robot = data.robot;
        data.isFull = false;
        data.isGameEnd = false;
        data.isGameStart = true;
        data.gameWinner = "";
        data.isRotateCamera = true;
        data.reset = false;
        data.cameraControll = true;
        data.turn = true;
        data.showGameMenu = false;
        data.showPause = false;
        data.btnshowMenu = false;
        data.btnResetPlay = false;
      }
      //Go Menu
      function goMenu() {
        //Clear Obg on plane
        for (var i = 0; i < 9; i++) {
          scene.remove(scene.getObjectByName("x"));
          scene.remove(scene.getObjectByName("o"));
        }
        scene.remove(scene.getObjectByName("textVictory"));

        lobby();

        //Reset Impostazioni
        for (var i = 0; i < 3; i++) {
          for (var j = 0; j < 3; j++) {
            data_tris[i][j] = 1;
          }
        }

        data.player = false;
        //data.robot = data.robot;
        data.isFull = false;
        data.isGameEnd = false;
        data.isGameStart = false;
        data.gameWinner = "";
        data.isRotateCamera = true;
        data.reset = false;
        data.cameraControll = true;
        data.turn = true;
        data.showGameMenu = true;
        data.showPause = false;
        data.btnshowMenu = false;
        data.btnResetPlay = false;
      }
      //Animation X and O
      function Func_anim_x_o() {
        if (!anim_x_o.length == 0) {
          for (var i = 0; i < anim_x_o.length; i++) {
            if (anim_x_o[i].position.y >= 0.12) {
              anim_x_o[i].position.y = anim_x_o[i].position.y - 0.01;
            }
          }
        }
      }
      //Other Methods
    },
  },
  components: {},
  computed: {},
};
</script>