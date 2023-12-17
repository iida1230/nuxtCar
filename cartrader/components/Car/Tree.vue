
<script lang="ts">

import {
    Disclosure,
    DisclosureButton,
    DisclosurePanel,
  } from '@headlessui/vue'

type B =   {id?:string,temp?:B[]}
type A =   {no:number,name:string,items?: B[] }
interface Props {
    item?:  A 
    index?:number
    items?:B
}
export default defineComponent({
   components:{
    Disclosure,
    DisclosureButton,
    DisclosurePanel,
   } ,
name: 'Tree',
props: {
    item: {type: Object as PropType<A>, default: undefined,},
    index: {type: Number, default: undefined,},
    items: {type: Object as PropType<B>, default: undefined,}
  },

  setup(props: Props, cotext) {
    
    const b = reactive({
        saa:ref()
    })
    const main = ref()
    const main2 = ref()
    const group = ref()
    const a ={id:"123"}

    const functions ={
        panelOpne : () =>{
            console.log("panelOpne")
            // 親コンポーネントイベント
            if(main.value){
                console.log("main?")
        main.value.el.click()
       }
       // 子コンポーネントイベント
       if(main2.value){
        console.log("main2?")
        main2.value.el.click()
        cotext.emit("ls-row")
       }
       
        }
    }

    onMounted(()=>{
        // console.log("id確認",props.items?.id)
        if(props.items?.id === "一層目3巡名一回目(ラスト)"){
            console.log("emit発火")
            functions.panelOpne()
            // cotext.emit("ls-row")
        }

    //    if(main.value){
    //     main.value.el.click()
    //    }
    //    if(group.value){
    //     group.value.el.click()
    //    }
      
        console.log("--------",main.value)}
        )
    return { a,main,group,main2,functions};
  },
});
</script>
<template >
     <template v-if="$props.index !== undefined">
   <div class="w-full px-4 pt-16">
    <div class="mx-auto w-full max-w-md rounded-2xl bg-white p-2">
      <Disclosure v-slot="{ open }" >
        <DisclosureButton ref="main"
          class="flex w-full justify-between rounded-lg bg-purple-100 px-4 py-2 text-left text-sm font-medium text-purple-900 hover:bg-purple-200 focus:outline-none focus-visible:ring focus-visible:ring-purple-500/75"
        >
          <span>{{ $props.item?.name }}</span>
          <ChevronUpIcon
            :class="open ? 'rotate-180 transform' : ''"
            class="h-5 w-5 text-purple-500"
          />
        </DisclosureButton>
        <Tree  v-for="e of $props.item?.items"  :items="e"  @ls-row="functions.panelOpne"></Tree>
      </Disclosure>
     
    </div>
  </div>
</template>
<template v-else>
    <DisclosurePanel  class="px-4 pb-2 pt-4 text-sm text-gray-500" :unmount="false">
          <Disclosure as="div" class="mt-2" v-slot="{ open }">
        <DisclosureButton  ref="main2"
          class="flex w-full justify-between rounded-lg bg-purple-100 px-4 py-2 text-left text-sm font-medium text-purple-900 hover:bg-purple-200 focus:outline-none focus-visible:ring focus-visible:ring-purple-500/75"
        >
        <span>{{ $props.items?.id }}nai?</span>
          <ChevronUpIcon
            :class="open ? 'rotate-180 transform' : ''"
            class="h-5 w-5 text-purple-500"
          />
        </DisclosureButton>
        <!--Templateの場合DisclosurePanelが表示されなくなる?-->
       <template v-if="$props.items?.temp">
        <Tree  v-for="e of $props.items?.temp"  :items="e"  @ls-row="functions.panelOpne"></Tree>
        </template>
      </Disclosure>

    </DisclosurePanel>
</template>
   

</template>


