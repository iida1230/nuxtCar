<script>
export default defineComponent({
  props: {
    features: Array,
  },
  setup(props) {
    console.log("props", props.features);
  },
});
</script>
<template>
  <div class="mr-10 mt-5 border-b pb-5">
    <div class="flex text-lg mt-2" v-for="faeture in features" :key="faeture">
      <p class="rounded text-lime-800 mr-3">×</p>
      <p>{{ faeture }}</p>
    </div>
  </div>
</template>