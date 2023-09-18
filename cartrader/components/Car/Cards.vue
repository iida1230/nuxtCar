
<script>
export default defineComponent({
  setup() {
    const { cars } = useCars();

    const favorite = useLocalStorage("favored", {});
    const handleFavorite = (id) => {
      if (id in favorite.value) {
        console.log("id", id);
        delete favorite.value[id];
      } else {
        favorite.value = {
          ...favorite.value,
          [id]: true,
        };
      }
    };
    return { cars, handleFavorite, favorite };
  },
});
</script>
<template>
  <div class="w-full">
    <ClientOnly>
      <CarCard
        v-for="car in cars"
        :key="car.id"
        :car="car"
        @favor="handleFavorite"
        :favored="car.id in favorite"
      />
    </ClientOnly>
  </div>
</template>