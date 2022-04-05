<template>
  <div>
    <div
      class="absolute text-center mx-auto text-6xl"
      style="color: white; left: 50%; transform: translate(-50%, 0)"
    >
      {{ data.gameWinner }}
    </div>
      <button type="button" @click="data.isRotateCamera = !data.isRotateCamera" class="absolute  bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full" style="left:3px; bottom:3px;">
        Rotate Camera 
      </button>
      <button type="button" @click="data.robot = !data.robot" class="absolute  bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-full" style="right:3px; bottom:3px;">
       Robot {{data.robot}}
      </button>
  </div>
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
      data: {
        player: false,
        robot: true,
        isFull: false,
        isGameEnd: false,
        gameWinner: "",
        isRotateCamera: true,
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
    };
  },
  mounted() {
    this.init2();
    console.log("Mounted");
  },
  methods: {
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

      //Varibili globali
      var data_tris = this.data_tris;
      var map_tris = this.map_tris;
      var data = this.data;


      //var color
      var color_bg = new THREE.Color("rgb(132, 108, 91)");
      var color_plane = new THREE.Color("rgb(2, 52, 54)");
      var color_separe = new THREE.Color("rgb(17, 17, 17)");
      var color_borderPlane = new THREE.Color("rgb(0, 34, 35)");
      var color_x = new THREE.Color("rgb(254, 198, 1)");
      var color_o = new THREE.Color("rgb(183, 36, 92)");

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

        //Plane Separe
        //const separe = new THREE.BoxGeometry(3, 0.1, 0.05);
        const separe = new RoundedBoxGeometry(3.2, 0.15, 0.05, 8, 1);
        const materialSepare = new THREE.MeshLambertMaterial({
          color: color_separe,
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
        const borderPlane2 = new THREE.Mesh(
          geometry_borderPlane,
          materialBorderPlane
        );
        borderPlane2.position.z = -1.55;
        const borderPlane3 = new THREE.Mesh(
          geometry_borderPlane,
          materialBorderPlane
        );
        borderPlane3.position.x = -1.55;
        borderPlane3.rotation.y = Math.PI / 2;
        const borderPlane4 = new THREE.Mesh(
          geometry_borderPlane,
          materialBorderPlane
        );
        borderPlane4.position.x = 1.55;
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
              // color: Math.random() *     0xffffff,
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
              console.log("Click obg " + ev.data.target.name);
              if (!data.isGameEnd) placeObj(ev.data.target.name);
              if(!data.isGameEnd && data.robot) playRobot();
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
        controls.enableZoom = true;
        controls.autoRotate = data.isRotateCamera;
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
        if (data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] == 1) {
          if (turn) {
            var obg = createX();
            data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] = 5;
          } else {
            var obg = createO();
            data_tris[map_tris[k].charAt(0)][map_tris[k].charAt(1)] = 3;
          }
          turn = !turn; //change turn
          //Position obg
          obg.position.y = 0.1;
          obg.position.x = position[0];
          obg.position.z = position[1];
          scene.add(obg);
        }
        // this.controll(); //controll winner
        controll();
      }

      //Object X
      function createX() {
        var altX = 0;
        const geometryX = new THREE.BoxGeometry(0.7, 0.15, 0.08);
        const materialX = new THREE.MeshLambertMaterial({ color: color_x });
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
        const materialO = new THREE.MeshLambertMaterial({ color: color_o });
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
      //Method for controll winner
      function controll() {
        //Controllo Righe
        for (var i = 0; i < 3; i++) {
          var addRig = 1;
          for (var j = 0; j < 3; j++) {
            addRig = data_tris[i][j] * addRig;
          }
          if (addRig == 27) {
            data.gameWinner = "Winner O";
          }
          if (addRig == 125) {
            data.gameWinner = "Winner X";
          }
        }
        //Controllo Colonne
        for (var i = 0; i < 3; i++) {
          var addCol = 1;
          for (var j = 0; j < 3; j++) {
            addCol = data_tris[j][i] * addCol;
          }
          if (addCol == 27) {
            data.gameWinner = "Winner O";
          }
          if (addCol == 125) {
            data.gameWinner = "Winner X";
          }
        }
        //Controllo Diagonale Sinistra
        var addDig1 = 1;
        for (var i = 0; i < 3; i++) {
          addDig1 = data_tris[i][i] * addDig1;
          if (addDig1 == 27) {
            data.gameWinner = "Winner O";
          }
          if (addDig1 == 125) {
            data.gameWinner = "Winner X";
          }
        }
        //Controllo Diagonale Destra
        var addDig2 = data_tris[0][2] * data_tris[1][1] * data_tris[2][0];
        if (addDig2 == 27) {
          data.gameWinner = "Winner O";
        }
        if (addDig2 == 125) {
          data.gameWinner = "Winner X";
        }
        //Final Controll
        if (data.gameWinner == "") {
          if (data.isFull) {
            data.gameWinner = "Parity";
            data.isGameEnd = true;
          }
        } else {
          data.isGameEnd = true;
        }
        console.log(data.isGameEnd);
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
            console.log(calcRiga)
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
/*
      //Play Diagonal Sinistra
      if (isValuePut) {
        var calcDiagonalSx =
          this.data_tris[0][0] * this.data_tris[1][1] * this.data_tris[2][2];
        if (calcDiagonalSx == 9) {
          for (var j = 0; j < 3; j++) {
            if (this.data_tris[j][j] == 1) {
              this.selectCasella(this.getPositionTrisMap(j, j), 3);
              isValuePut = false;
            }
          }
        }
      }

      //Play Diagonal Destra
      if (isValuePut) {
        var calcDiagonalSx =
          this.data_tris[0][2] * this.data_tris[1][1] * this.data_tris[2][0];
        if (calcDiagonalSx == 9) {
          var i = 0;
          var j = 2;
          for (var k = 0; k < 3; k++) {
            if (this.data_tris[i][j] == 1) {
              this.selectCasella(this.getPositionTrisMap(i, j), 3);
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
            calcRiga = this.data_tris[i][j] * calcRiga;
          }
          if (calcRiga == 25) {
            for (var j = 0; j < 3; j++) {
              if (this.data_tris[i][j] == 1) {
                this.selectCasella(this.getPositionTrisMap(i, j), 3);
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
            calcColum = this.data_tris[j][i] * calcColum;
          }
          if (calcColum == 25) {
            for (var j = 0; j < 3; j++) {
              if (this.data_tris[j][i] == 1) {
                this.selectCasella(this.getPositionTrisMap(j, i), 3);
                isValuePut = false;
              }
            }
          }
        }
      }

      //Play Diagonal Sinistra
      if (isValuePut) {
        var calcDiagonalSx =
          this.data_tris[0][0] * this.data_tris[1][1] * this.data_tris[2][2];
        if (calcDiagonalSx == 25) {
          for (var j = 0; j < 3; j++) {
            if (this.data_tris[j][j] == 1) {
              this.selectCasella(this.getPositionTrisMap(j, j), 3);
              isValuePut = false;
            }
          }
        }
      }

      //Play Diagonal Destra
      if (isValuePut) {
        var calcDiagonalSx =
          this.data_tris[0][2] * this.data_tris[1][1] * this.data_tris[2][0];
        if (calcDiagonalSx == 25) {
          var i = 0;
          var j = 2;
          for (var k = 0; k < 3; k++) {
            if (this.data_tris[i][j] == 1) {
              this.selectCasella(this.getPositionTrisMap(i, j), 3);
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
        console.log(this.data_tris[1][1] == 1);
        if (this.data_tris[1][1] == 1)
          if (
            this.data_tris[0][0] == 5 ||
            this.data_tris[0][2] == 5 ||
            this.data_tris[2][0] == 5 ||
            this.data_tris[2][2] == 5
          ) {
            console.log("XXX");
            this.selectCasella(this.getPositionTrisMap(1, 1), 3);
            isValuePut = false;
          }
      }
*/
      //Position Random
      if (isValuePut && !this.isFull) {
        while (random) {
          var numRandom = Math.floor(Math.random() * 10);
          if (render_tris[numRandom] == 1) {
            placeObj();
            random = false;
            isValuePut = false;
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
      //Other Methods
    },
  },
  components: {},
  computed: {},
};
</script>