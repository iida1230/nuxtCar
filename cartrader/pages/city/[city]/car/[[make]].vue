<!--NuxtPageでレンダリング-->

<script>
import { defineComponent } from "@vue/composition-api";

export default defineComponent({
  setup() {
    console.log("make.vue");
  },
});
</script>

<template>
  <div>
    <CarCards />
  </div>
</template>



<!-- <NavBar2 />

  <div
    class="mx-auto mt-4 max-w-7xl space-y-4 px-4 xs:px-8 sm:px-10 lg:px-16 pb-16 w-3/5"
  >
    <div class="mt-32 flex">
      <CarSideBar />

      <NuxtPage />
    </div>
  </div> -->