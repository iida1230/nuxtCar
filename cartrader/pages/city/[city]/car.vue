<script>
definePageMeta({
  layout: "custom",
});
export default defineComponent({
  setup() {
    console.log("car.vue");
    const route = useRoute();
    useHead({
      title: `${route.params.make} in ${route.params.city}`,
    });
  },
});
</script>
<template>
  <div>
    <div class="mt-32 flex">
      <NuxtErrorBoundary>
        <CarSideBar />
        <!--NuxtPageは現在のページのファイルがレンダリングされる-->
        <NuxtPage />
        <template #error="{ error }">
          <div class="text-center mx-auto flex flex-col">
            <code>{{ error }}</code>
            <button
              class="rounded mt-7 text-2xl bg-blue-400 px-7 py-4 text"
              @click="error.value = null"
            >
              Go Back
            </button>
          </div>
        </template>
      </NuxtErrorBoundary>
    </div>
  </div>
</template>