<!--NuxtPageでレンダリング-->

<script lang="ts">
import draggable from "vuedraggable";
import Sortable, { MultiDrag, Swap } from "sortablejs";

import Tree from '~/components/Car/Tree.vue'

type B =   {id?:string,temp?:B[]} 
type A =   {no:number,name:string,items?: B[]}
export default defineComponent({
  components: {
    draggable,
    Tree
  },
  setup() {
    console.log("make.vue");
    interface state {
      files:[]
      files2:[]
    item: A []
    options:[]
    isDrag:boolean
    testRef:any
    testRef2:any
}
  
    const state:state = reactive({
      isDrag: false,
      files: [],
      files2: [],
      item: [
        { no: 1, name: "キャベツ", items: [
          {
            id:"一層目1巡名一回目",
            temp:[
              {
                id:"一層目2巡名一回目",
                temp:[
                  {
                    id:"一層目3巡名一回目(ラスト)"
                  }
                ]
              },
              {
                id:"1層2層目2巡名一回目",
                temp:[
                  {
                    id:"1層2層目3巡名一回目(ラスト)"
                  }
                ]
              }
            ]
          },
          {
            id:"2層目1巡名一回目",
            temp:[
              {
                id:"2層目2巡(ラスト)"
              }
            ]
          }
        ]},
        { no: 2, name: "ステーキ", items: [{temp:[{id:"a1"}]},{temp:[{id:"a2"}] }]},
        { no: 3, name: "リンゴ",  items: [{temp:[{id:"a3"}]},{temp:[{id:"a4"}] }]},
      ],
      testRef: ref(null),
      testRef2: ref(null),
      options: {
        group: "board", // group オプションを指定した draggable 間はDrag and Dropで要素を移すことができる
        animation: 100, // animationオプションを指定すると ソート時に 値(ms)で アニメーションされる
        multiDragKey: "CTRL",
        multiDrag: true,
        selectedClass: "selected",
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
      dropFile: (event: { dataTransfer: { files: any; }; }) => {
        state.isDrag = false;
        state.files = [...state.files, ...event.dataTransfer.files];
        console.log(state.files);
      },
    };
    onBeforeMount(() => {
      state.testRef = document.getElementById("test2");
      state.testRef2 = document.getElementById("test3");
      console.log(state.testRef);
      Sortable.mount(new MultiDrag());
      state.options = Sortable.create(state.testRef, {
        group: "shared",
        multiDrag: true,
        multiDragKey: "CTRL",
        sort:false,
        selectedClass: "selected",
        animation: 150,
      });
       Sortable.create(state.testRef2, {
        group: "shared",
        multiDrag: true,
        multiDragKey: "CTRL",
        sort:false,
        selectedClass: "selected",
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
    <!-- <CarCards /> -->
      <Tree  v-for="(it,index) of state.item"  :index="index"  :item="(it as A)" />
 
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
         :sort="false"
          ghost-class=""
          tag="div"
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

      <div
        class="border w-full h-full flex flex-row"
        :class="{ 'bg-gray-700': state.isDrag }"
      >
        <draggable
          id="test3"
          class="w-full h-full flex justify-center items-center test"
          v-model="state.files2"
          item-key="no"
         :sort="false"
          ghost-class=""
          tag="div"
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
<style>


.selected {
  background-color: #f9c7c8 !important;
  border: solid red 1px !important;
  z-index: 1 !important;
}

</style>



<!-- <NavBar2 />

  <div
    class="mx-auto mt-4 max-w-7xl space-y-4 px-4 xs:px-8 sm:px-10 lg:px-16 pb-16 w-3/5"
  >
    <div class="mt-32 flex">
      <CarSideBar />

      <NuxtPage />
    </div>

function onBeforeMount(arg0: () => void) {
  throw new Error("Function not implemented.");
}
  </div> -->