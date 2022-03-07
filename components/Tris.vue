<template>
  <div class="container mx-auto px-4 h-screen flex items-center justify-center">
    <div class="square" style="">
      <!-- Content Square -->
      <div class="contentSquare grid grid-cols-3">
        <!-- Square 1 -->
        <div
          @click="premuto(1)"
          class="square1 border-t-8 border-l-8 rounded-l-xl"
        >
          <img src="~/assets/img/x.svg" width="90%" class="absolute"/>
        </div>
        <div @click="premuto(2)" class="square1 border-t-8 border-l-8">2</div>
        <div
          @click="premuto(3)"
          class="square1 border-t-8 border-l-8 border-r-8 rounded-r-xl"
        >
          3
        </div>
        <div @click="premuto(4)" class="square1 border-t-8 border-l-8">4</div>
        <div @click="premuto(5)" class="square1 border-t-8 border-l-8">5</div>
        <div
          @click="premuto(6)"
          class="square1 border-t-8 border-l-8 border-r-8"
        >
          6
        </div>
        <div
          @click="premuto(7)"
          class="square1 border-t-8 border-l-8 border-b-8 rounded-l-xl"
        >
          7
        </div>
        <div
          @click="premuto(8)"
          class="square1 border-t-8 border-l-8 border-b-8"
        >
          8
        </div>
        <div
          @click="premuto(9)"
          class="
            square1
            border-t-8 border-l-8 border-b-8 border-r-8
            rounded-r-xl
          "
        >
          9
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "tris",
  data() {
    return {};
  },
  methods: {
    premuto(x) {
      console.log(x);
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
