
<script>
import heartFilled from "@/assets/heartFilled.png";
import heartOutline from "@/assets/heartOutline.png";
export default defineComponent({
  props: {
    car: "",
    favored: Boolean,
  },
  emits: ["favor", "delete"],
  setup(props) {
    // const emit = defineEmits(["favor"]);

    // const favored = useState(`favored-${props.car.id}`, () => {
    //   console.log("favored");
    //   return false;
    // });
    console.log("sss", props.car);
    const test = () => {
      console.log(`/car/${props.car.name}-${props.car.id}`);
      navigateTo(`/car/${props.car.name}-${props.car.id}`);
    };
    return { heartFilled, heartOutline, test };
  },
});
</script>

<template>
  <!-- CAR CARD -->
  <div
    class="relative shadow border w-full overflow-hidden mb-5 cursor-pointer h-[200px]"
  >
    <img
      class="absolute w-7 right-5 top-2 z-20"
      :src="favored ? heartFilled : heartOutline"
      alt=""
      @click="$emit('favor', car.id)"
    />
    <div class="flex h-full" @click="test">
      <NuxtImg :src="car.url" alt="" class="w-[300px] h-full" />
      <div class="p-4 flex flex-col">
        <div>
          <h1 class="text-2xl text-blue-700">{{ car.name }}</h1>
          <p class="text-gray-700">{{ car.description }}</p>
        </div>
        <h1 class="mt-auto text-xl">{{ car.price }}</h1>
      </div>
    </div>
  </div>
  <!-- CAR CARD -->
</template>