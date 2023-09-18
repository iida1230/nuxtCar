
import { CarSearchBar } from '#build/components';
<script>
import { defineComponent } from "@vue/composition-api";

export default defineComponent({
  setup() {},
});
</script>

<template>
  <!-- HOME HERO-->

  <div
    class="relative h-screen bg-no-repeat bg-cover bg-bottom bg-[url(https://picsum.photos/id/237/200/300)]"
  >
    <div class="z-10 absolute w-full h-full flex justify-center items-center">
      <div class="text-center w-1/2">
        <h1 class="text-8xl font-serif mb-10 text-white">Find your drive</h1>
        <!--HOME SEARCH BAR-->
        <CarSearchBar />
        <!--HOME SEARCH BAR-->
      </div>
    </div>
    <div class="z-1 absolute w-full h-full bg-black opacity-50">ccccccc</div>
  </div>
  <!-- HOME HERO-->
</template>
  