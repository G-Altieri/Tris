<template>
  <div class="container mx-auto px-4 h-screen flex items-center justify-center">
    <!-- Info Winner -->
    <div
      class="
        absolute
        h-screen
        z-50
        w-screen
        flex flex-col flex-nowrap
        justify-center
        items-stretch
      "
      v-show="isGameEnd"
    >
      <div
        class="
          text-6xl
          md:text-7xl
          lg:text-9xl
          textVictory
          flex
          items-center
          justify-center
          bgVictory
        "
      >
        {{ gameWinner }}
      </div>
      <!-- Button Reset -->
      <div class="flex items-center justify-center pt-6 bgVictory pb-3">
        <button
          class="
            bg-white
            hover:bg-gray-100
            text-gray-800
            font-semibold
            py-2
            px-4
            border border-gray-400
            rounded
            shadow
          "
          @click="resetTris()"
        >
          RESET
        </button>
        <NuxtLink
          class="
            bg-white
            hover:bg-gray-100
            text-gray-800
            font-semibold
            py-2
            px-4
            border border-gray-400
            rounded
            shadow
            mx-6
          "
        to="/"
        >
          HOME
        </NuxtLink>
      </div>
    </div>
    <!-- Tris Game -->
    <div class="square" :class="{ blur: isGameEnd }" style="">
      <!-- Content Square -->
      <div class="contentSquare grid grid-cols-3">
        <!-- Square playing Tris -->
        <div v-for="i in 9" :key="i">
          <div @click="clicked(i)" :class="classSquareTris[i]">
            <img
              src="~/assets/img/x.svg"
              width="80%"
              class="absolute animX"
              v-if="render_tris[i] == 5"
            />
            <img
              src="~/assets/img/o.svg"
              width="80%"
              class="absolute animO"
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
      isFull: false,
      isGameEnd: false,
      gameWinner: "",
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
      //Controllo se il gioco e finito
      if (!this.isGameEnd) {
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
              this.selectCasella(x, 3);
            } else {
              //Metto X
              this.selectCasella(x, 5);
            }
            //Change player
            this.player = !this.player;
            //Controll winner
            this.controll();
          } else {
            //Controllo a che giocatore tocca

            //Metto X
            this.selectCasella(x, 5);
            //Controll winner
            this.controll();
            if (!this.isGameEnd) {
              //Mette O
              this.playRobot(x);
            }
            //Controll winner
            this.controll();
          }
        }
      }
    },
    //Method for AI Robot
    playRobot() {
      console.log("Gioca il robot");

      var random = true;
      var isValuePut = true;

      //MOSSE VINCENTI
      //Play Rows
      if (isValuePut) {
        for (var i = 0; i < 3; i++) {
          var calcRiga = 1;
          for (var j = 0; j < 3; j++) {
            calcRiga = this.data_tris[i][j] * calcRiga;
          }
          if (calcRiga == 9) {
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
          if (calcColum == 9) {
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

      //Position Random
      if (isValuePut && !this.isFull) {
        while (random) {
          var numRandom = Math.floor(Math.random() * 10);
          if (this.render_tris[numRandom] == 1) {
            this.selectCasella(numRandom, 3);
            random = false;
            isValuePut = false;
          }
        }
      }
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
          this.gameWinner = "Winner O";
        }
        if (addRig == 125) {
          this.gameWinner = "Winner X";
        }
      }
      //Controllo Colonne
      for (var i = 0; i < 3; i++) {
        var addCol = 1;
        for (var j = 0; j < 3; j++) {
          addCol = this.data_tris[j][i] * addCol;
        }
        if (addCol == 27) {
          this.gameWinner = "Winner O";
        }
        if (addCol == 125) {
          this.gameWinner = "Winner X";
        }
      }
      //Controllo Diagonale Sinistra
      var addDig1 = 1;
      for (var i = 0; i < 3; i++) {
        addDig1 = this.data_tris[i][i] * addDig1;
        if (addDig1 == 27) {
          this.gameWinner = "Winner O";
        }
        if (addDig1 == 125) {
          this.gameWinner = "Winner X";
        }
      }
      //Controllo Diagonale Destra
      var addDig2 =
        this.data_tris[0][2] * this.data_tris[1][1] * this.data_tris[2][0];
      if (addDig2 == 27) {
        this.gameWinner = "Winner O";
      }
      if (addDig2 == 125) {
        this.gameWinner = "Winner X";
      }

      if (this.gameWinner == "") {
        if (this.isFull) {
          this.gameWinner = "Parity";
          this.isGameEnd = true;
        }
      } else {
        this.isGameEnd = true;
      }
      console.log(this.gameWinner);
    },
    //Position Numeber da i, j
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
    //Select
    selectCasella(x, valueGame) {
      var i, j;
      switch (x) {
        case 1:
          i = 0;
          j = 0;
          break;
        case 2:
          i = 0;
          j = 1;
          break;
        case 3:
          i = 0;
          j = 2;
          break;
        case 4:
          i = 1;
          j = 0;
          break;
        case 5:
          i = 1;
          j = 1;
          break;
        case 6:
          i = 1;
          j = 2;
          break;
        case 7:
          i = 2;
          j = 0;
          break;
        case 8:
          i = 2;
          j = 1;
          break;
        case 9:
          i = 2;
          j = 2;
          break;
      }

      this.data_tris[i][j] = valueGame;
      this.render_tris[x] = valueGame;

      //Controll is Full
      var find = false;
      for (var i = 0; i < 3 && !find; i++) {
        for (var j = 0; j < 3 && !find; j++) {
          if (this.data_tris[i][j] == 1) {
            find = true;
          }
        }
      }
      if (find) this.isFull = false;
      else this.isFull = true;
    },
    resetTris() {
      this.data_tris = [
        [1, 1, 1],
        [1, 1, 1],
        [1, 1, 1],
      ];
      this.render_tris = {
        1: 1,
        2: 1,
        3: 1,
        4: 1,
        5: 1,
        6: 1,
        7: 1,
        8: 1,
        9: 1,
      };
      this.isFull = false;
      this.isGameEnd = false;
      this.gameWinner = "";
    },
  },
  mounted() {},
  components: {},
  computed: {},
};
</script>

<style>
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
  border-color: #050401;
  background-color: #540D6E;
  height: 100%;
  box-sizing: content-box;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 200%;
  position: relative;
}
.square1:hover {
  background-color: #FFD23F;
}
.blur {
  filter: blur(13px);
  -webkit-filter: blur(13px);
}
.bgVictory{
  background-color: rgba(17, 75, 95,0.75);
}
.textVictory{
  color: #F42272;
}


.animO{
    animation: hideshow 0.5s ease-in-out ;
 /*stroke-dasharray: 227;
  stroke-dashoffset: 0;*/
}
.animX{
    animation: hideshow 0.5s ease-in-out ;

}
.bgTris{
  background-color: #050401;
}
@keyframes hideshow {
  0% { opacity: 0;
   transform: scale(0);
  }
  50%{
    opacity: 1;
    transform: scale(1.2);
  }
  100% { opacity: 1;
  transform: scale(1); }
} 

</style>

