<script>
export default defineComponent({
  setup() {
    const { makes } = useCars();
    const route = useRoute();
    const router = useRouter();
    const state = reactive({
      modal: {
        make: false,
        location: false,
        price: false,
      },
      priceRange: {
        min: "",
        max: "",
      },
      city: "",

      // car: computed(() => {
      //   return cars.find((c) => {
      //     console.log(c);
      //     return c.id === parseInt(route.params.id);
      //   });
      // }),
      // cityError: false,
    });

    const priceRangeText = computed(() => {
      const minPrice = route.query.minPrice;
      const maxPrice = route.query.maxPrice;

      if (!minPrice && !maxPrice) return "Any";
      else if (!minPrice && maxPrice) {
        return `< $${maxPrice}`;
      } else if (minPrice && !maxPrice) {
        return `> $${minPrice}`;
      } else {
        return `$${minPrice}-$${maxPrice}`;
      }
    });
    const updateModal = (key) => {
      state.modal[key] = !state.modal[key];
    };
    const onChangeLoction = () => {
      if (!state.city) return;
      if (!isNaN(parseInt(state.city))) {
        throw createError({
          statusCode: 400,
          message: `INvalid city format　${route.params.id}`,
        });
      }
      updateModal("location");
      navigateTo(`/city/${state.city}/car/${route.params.make}`);
      state.city = "";
    };

    const onChangeMake = (make) => {
      updateModal("make");
      navigateTo(`/city/${route.params.city}/car/${make}`);
    };

    const onChangePrice = () => {
      updateModal("price");
      if (state.priceRange.max && state.priceRange.min) {
        if (state.priceRange.min > state.priceRange.max) return;
      }
      router.push({
        query: {
          minPrice: state.priceRange.min,
          maxPrice: state.priceRange.max,
        },
      });
    };

    return {
      state,
      makes,
      route,
      updateModal,
      onChangeLoction,
      onChangeMake,
      priceRangeText,
      onChangePrice,
    };
  },
});
</script>
<template>
  <!-- CAR SIDE BAR -->
  <div class="shadow border w-64 mr-10 z-30 h-[190px]">
    <!--LOCATION START-->
    <div class="p-5 flex justify-between relative cursor-pointer border-b">
      <h3>Location</h3>
      <h3 @click="updateModal('location')" class="text-blue-400 capitalize">
        {{ route.params.city }}
      </h3>
      <div
        v-if="state.modal.location"
        class="absolute border shadow left-60 p-5 top-1 -m-1 bg-white"
      >
        <input type="text" class="border p-1 rounded" v-model="state.city" />
        <button
          @click="onChangeLoction"
          class="bg-blue-400 w-full mt-2 rounded text-white p-1"
        >
          Apply
        </button>
      </div>
    </div>
    <!--LOCATION END-->

    <!--MAKE START-->
    <div class="p-5 flex justify-between relative cursor-pointer border-b">
      <h3>Make</h3>
      <h3 class="text-blue-400 capitalize" @click="updateModal('make')">
        {{ route.params.make || "Any" }}
      </h3>
      <div
        v-if="state.modal.make"
        class="absolute border shadow left-60 p-5 top-1 -m-1 w-[600px] flex justify-between flex-wrap bg-white"
      >
        <h4
          v-for="make in makes"
          :key="make"
          class="w-1/3"
          @click="onChangeMake(make)"
        >
          {{ make }}
        </h4>
      </div>
    </div>
    <!--MAKE END-->

    <!--PRICE START-->
    <div class="p-5 flex justify-between relative cursor-pointer border-b">
      <h3>Price</h3>
      <h3 class="text-blue-400 capitalize" @click="updateModal('price')">
        {{ priceRangeText }}
      </h3>
      <div
        class="absolute border shadow left-60 p-5 top-1 -m-1 bg-white"
        v-if="state.modal.price"
      >
        <input
          class="border p-1 rounded"
          type="number"
          placeholder="Min"
          v-model="state.priceRange.min"
        />
        <input
          class="border p-1 rounded"
          type="number"
          placeholder="Min"
          v-model="state.priceRange.max"
        />
        <button
          class="bg-blue-400 w-full mt-2 rounded text-white p-1"
          @click="onChangePrice"
        >
          Apply
        </button>
      </div>
    </div>

    <!--PRICE END-->
  </div>
</template>