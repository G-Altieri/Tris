<template>
  <div class="min-h-screen bgTris">
    <div class="text-6xl md:text-7xl lg:text-9xl font-medium text-white flex justify-center">TRIS</div>
   <div class="flex flex-col items-center justify-center">
     
    <NuxtLink
      to="/tris2d"
      class="text-white text-center mt-20 border-4 border-lime-500 rounded-lg w-32"
      >TRIS 2D</NuxtLink
    >
    <NuxtLink
      to="/tris3d"
      class="text-white text-center mt-20 border-4 border-lime-500 rounded-lg w-32"
      >TRIS 3D</NuxtLink
    >
   </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
};
</script>

<style>
.bgTris {
  background-color: #050401;
}
</style>
