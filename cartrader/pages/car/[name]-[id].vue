<script>
definePageMeta({
  layout: "custom",
});
export default defineComponent({
  build: {
    transpile: ["class-validator"],
  },
  vite: {
    optimizeDeps: {
      exclude: ["class-validator"],
    },
  },
  setup() {
    const route = useRoute();
    const { cars } = useCars();
    useHead({
      title: `${route.params.name} in ${route.params.id}`,
    });
    const state = reactive({
      car: computed(() => {
        return cars.find((c) => {
          console.log(c);
          return c.id === parseInt(route.params.id);
        });
      }),
      cityError: false,
    });

    if (!state.car) {
      console.log("ID エラー");
      throw createError({
        statusCode: 404,
        message: `ID ないよ　${route.params.id}`,
      });
    } else {
      console.log("state.car", state.car);
    }
    return { state };
  },
});
</script>
<template>
  <div v-if="state.car">
    <CarDetailHero :car="state.car" />
    <CarDetailAttribute :features="state.car.features" />
    <CarDetailDescription :description="state.car.description" />
    <CarDetailContact />
  </div>
</template>