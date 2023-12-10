<!--NuxtPageでレンダリング-->

<script>
import { defineComponent } from "@vue/composition-api";
import draggable from "vuedraggable";
import Sortable, { MultiDrag, Swap } from "sortablejs";
export default defineComponent({
  components: {
    draggable,
  },
  setup() {
    console.log("make.vue");
    const state = reactive({
      isDrag: false,
      files: [],
      items: [
        { no: 1, name: "キャベツ", categoryNo: "1" },
        { no: 2, name: "ステーキ", categoryNo: "2" },
        { no: 3, name: "リンゴ", categoryNo: "3" },
      ],
      testRef: ref(null),
      options: {
        group: "board", // group オプションを指定した draggable 間はDrag and Dropで要素を移すことができる
        animation: 100, // animationオプションを指定すると ソート時に 値(ms)で アニメーションされる
        multiDragKey: "CTRL",
        multiDrag: true,
        selectedClass: "selected-----------",
      },
      op: computed(() => {
        return state.options;
      }),
    });

    const on = {
      dragEnter: () => {
        state.isDrag = true;
      },
      dragLeave: () => {
        state.isDrag = false;
      },
      dropFile: (event) => {
        state.isDrag = false;
        state.files = [...state.files, ...event.dataTransfer.files];
        console.log(state.files);
      },
    };
    onBeforeMount(() => {
      state.testRef = document.getElementById("test2");
      console.log(state.testRef);
      Sortable.mount(new MultiDrag());
      state.options = Sortable.create(state.testRef, {
        group: "shared----------",
        multiDrag: true,
        multiDragKey: "CTRL",
        selectedClass: "selected-----------",
        animation: 150,
      });
      nextTick();
      console.log(state.testRef);
      console.log(state.options);
    });

    return { state, on };
  },
});
</script>

<template>
  <div>
    <div id="test1" class="list-group">
      <div class="list-group-item">Item 1</div>
      <div class="list-group-item">Item 2</div>
      <div class="list-group-item">Item 3</div>
      <div class="list-group-item">Item 4</div>
      <div class="list-group-item">Item 5</div>
    </div>
    <CarCards />
    <div class="flex w-full justify-center items-center h-[100px]">
      <!-- <div
        class="border w-full h-full flex flex-row"
        @dragenter="on.dragEnter"
        @dragleave="on.dragLeave"
        @dragover.prevent
        @drop.prevent="on.dropFile"
        :class="{ 'bg-gray-700': state.isDrag }"
      >
        <div
          v-for="file in state.files"
          :key="file"
          class="h-full flex justify-center items-center"
        >
          <NuxtImg
            :src="`https://placehold.jp/300x200.png`"
            alt=""
            class="h-auto"
          />
        </div>
      </div>
    </div> -->
      <div
        class="border w-full h-full flex flex-row"
        :class="{ 'bg-gray-700': state.isDrag }"
      >
        <draggable
          id="test2"
          class="w-full h-full flex justify-center items-center test"
          v-model="state.files"
          item-key="no"
          v-bind="state.op"
          :sort="false"
          ghost-class="ghost-----------"
          tag="div"
          @start="drag = true"
          @end="drag = false"
          @dragenter="() => on.dragEnter"
          @dragleave="() => on.dragLeave"
          @dragover.prevent
          @drop.prevent="on.dropFile"
          @drag="() => console.log('drag')"
        >
          <template #item="{ element }">
            <div id="list">
              <NuxtImg
                :src="`https://placehold.jp/300x200.png`"
                alt=""
                class="h-auto"
              />
              <div>{{ element.name }}</div>
            </div>
          </template>
        </draggable>
      </div>
    </div>
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