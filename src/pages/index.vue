<script setup>

</script>

<template>
  <main class="background" h-screen px-6 pt-6>
    <!-- light/dark toggle -->
    <div flex justify-end mb-5>
      <div @click="toggleDark()">
        <div v-if="isDark" i-carbon-sun />
        <div v-else text-white i-carbon-moon />
      </div>
    </div>

    <!-- card images -->
    <div pl-12>
      <img src="/bg-card-back.png">
    </div>
    <div pr-14 absolute class="top-35 left-6">
      <img src="/bg-card-front.png">
    </div>
    <div absolute top-40 left-11>
      <img src="/card-logo.svg" class="w-[62px]">
    </div>
    <div absolute text-white text-xl tracking-widest left-11>
      9591 6489 6389 1011
    </div>
    <div class="w-[75%]" justify-between flex text-white text-xs absolute top-70 tracking-widest>
      <div ml-5>
        FELICIA LEIRE
      </div>
      <div>
        09/26
      </div>
    </div>
  </main>
</template>

<style scoped>
.background {
  background-image: url('/bg-main-mobile.png');
  background-repeat: no-repeat;
  background-size: 100%;
}
</style>

<route lang="yaml">
meta:
  layout: home
</route>
