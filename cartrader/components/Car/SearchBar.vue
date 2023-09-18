<script>
import { defineComponent } from "@vue/composition-api";

export default defineComponent({
  setup() {
    const state = reactive({
      city: "test",
      cityError: false,
    });

    console.log("初期朱里");
    const router = useRouter();
    const handleSerch = () => {
      if (!state.city) {
        return (state.cityError = true);
      }
      console.log("aaaaaaaaaaaaaaaaaaaa");
      router.push(`/city/${state.city}/car`);
    };
    return { state, handleSerch };
  },
});
</script>

<template>
  <!--HOME SEARCH BAR-->
  <div
    class="font-serif text-2xl rounded-full bg-white flex justify-between overflow-hidden drop-shadow-2xl mx-auto"
  >
    <input
      placeholder="やあ"
      type="text"
      class="py-3 px-5 w-full text-2xl rounded-full focus:outline-none"
      name=""
      id=""
      v-model="state.city"
      :class="state.cityError ? 'border-red-500 border' : ''"
    />
    <button class="bg-sky-500 px-10 text-white" @click="handleSerch">
      Search
    </button>
  </div>
  <!--HOME SEARCH BAR-->
</template>
  