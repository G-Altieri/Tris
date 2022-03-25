<template>
  <div class="container mx-auto px-4 h-screen flex items-center justify-center">
    <div class="square" style="">
      <!-- Content Square -->
      <div class="contentSquare grid grid-cols-3">
        <!-- Square playing Tris -->
        <div v-for="i in 9" :key="i">
          <div @click="clicked(i)" :class="classSquareTris[i]">
            <img
              src="~/assets/img/x.svg"
              width="80%"
              class="absolute"
              v-if="render_tris[i] == 5"
            />
            <img
              src="~/assets/img/o.svg"
              width="80%"
              class="absolute"
              v-if="render_tris[i] == 3"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "tris",
  data() {
    return {
      player: false,
      robot: true,
      classSquareTris: {
        1: "square1 border-t-8 border-l-8 rounded-l-xl",
        2: "square1 border-t-8 border-l-8",
        3: "square1 border-t-8 border-l-8 border-r-8 rounded-r-xl",
        4: "square1 border-t-8 border-l-8",
        5: "square1 border-t-8 border-l-8",
        6: "square1 border-t-8 border-l-8 border-r-8",
        7: "square1 border-t-8 border-l-8 border-b-8 rounded-l-xl",
        8: "square1 border-t-8 border-l-8 border-b-8",
        9: "square1 border-t-8 border-l-8 border-b-8 border-r-8 rounded-r-xl",
      },
      tris: {
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
      //resTris: { r1: 1, r2: 1, r3: 1, c1: 1, c2: 1, c3: 1, d1: 1, d2: 1 },
      data_tris: [
        [1, 1, 1],
        [1, 1, 1],
        [1, 1, 1],
      ],
      render_tris: {
        1: 1,
        2: 1,
        3: 1,
        4: 1,
        5: 1,
        6: 1,
        7: 1,
        8: 1,
        9: 1,
      },
    };
  },
  methods: {
    clicked(x) {
      //Prendo la posizione del click
      var i = this.tris[x].charAt(0);
      var j = this.tris[x].charAt(1);
      //Controllo che gia non è stato gia premuto
      if (this.data_tris[i][j] == 1) {
        //Controllo se gioca il pc o un umano
        if (!this.robot) {
          //Controllo a che giocatore tocca
          if (this.player) {
            //Metto O
            this.data_tris[i][j] = 3;
            this.render_tris[x] = 3;
          } else {
            //Metto X
            this.data_tris[i][j] = 5;
            this.render_tris[x] = 5;
          }
          //Change player
          this.player = !this.player;
          //Controll winner
          this.controll();
        } else {
          //Controllo a che giocatore tocca
          if (this.player) {
          } else {
            //Metto X
            this.data_tris[i][j] = 5;
            this.render_tris[x] = 5;
            //Mette O
            this.playRobot(x);
          }
          //Controll winner
          this.controll();
        }
      }
    },
    //Method for AI Robot
    playRobot(x) {
      console.log("Gioca il robot");

      //Play Rows 
      for (var i = 0; i < 3; i++) {
        var calcRiga = 1;
        for (var j = 0; j < 3; j++) {
          calcRiga = this.data_tris[i][j] * calcRiga;
        }
        if (calcRiga == 25) {
          for (var j = 0; j < 3; j++) {
            if (this.data_tris[i][j] == 1) {
              this.data_tris[i][j] = 3;
              this.render_tris[this.getPositionTrisMap(i, j)] = 3;
            }
          }
        }
      }

      //Play Columns
    },
    //Method for controll winner
    controll() {
      //Controllo Righe
      for (var i = 0; i < 3; i++) {
        var addRig = 1;
        for (var j = 0; j < 3; j++) {
          addRig = this.data_tris[i][j] * addRig;
        }
        if (addRig == 27) {
          console.log("Winner O");
        }
        if (addRig == 125) {
          console.log("Winner X");
        }
      }
      //Controllo Colonne
      for (var i = 0; i < 3; i++) {
        var addCol = 1;
        for (var j = 0; j < 3; j++) {
          addCol = this.data_tris[j][i] * addCol;
        }
        if (addCol == 27) {
          console.log("Winner O");
        }
        if (addCol == 125) {
          console.log("Winner X");
        }
      }
      //Controllo Diagonale Sinistra
      var addDig1 = 1;
      for (var i = 0; i < 3; i++) {
        addDig1 = this.data_tris[i][i] * addDig1;
        if (addDig1 == 27) {
          console.log("Winner O");
        }
        if (addDig1 == 125) {
          console.log("Winner X");
        }
      }
      //Controllo Diagonale Destra
      var addDig2 =
        this.data_tris[0][2] * this.data_tris[1][1] * this.data_tris[2][0];
      if (addDig2 == 27) {
        console.log("Winner O");
      }
      if (addDig2 == 125) {
        console.log("Winner X");
      }
    },
    getPositionTrisMap(i, j) {
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
    },
  },
  mounted() {},
  components: {},
  computed: {},
};
</script>

<style scoped>
.square {
  position: relative;
  width: 90%;
  max-height: 90vh;
  max-width: 90vh;
}

.square:after {
  content: "";
  display: block;
  padding-bottom: 100%;
}

.contentSquare {
  position: absolute;
  width: 100%;
  height: 100%;
}
.square1 {
  border-color: rgb(0, 0, 0);
  background-color: rgb(107, 47, 102);
  height: 100%;
  box-sizing: content-box;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 200%;
  position: relative;
}
.square1:hover {
  background-color: yellow;
}
</style>

