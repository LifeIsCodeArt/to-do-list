<script setup>
import {computed, defineProps, ref, inject} from "vue";
const props = defineProps({
  value: {
    type: Object,
    default: () => {}
  },
  itemValue: {
    type: Array,
  },
  modelValue: Number
})

defineEmits(["modelValue",'deleteList','dragDropEvent','moveListToTheRight','moveListToTheLeft'])

const setCurrentTaskEl = (id) => {
  if(localStorage.getItem('setCurrentTaskBoardEl') !== id) {
    console.log('This id currently used')
  }
  else{
    localStorage.setItem('setCurrentTaskBoardEl',id.toString())
  }
}

const setNewTask = ref(true)
const setNewColor = ref(true)
const newTaskValue = ref('')
const activeColor = ref(1)

const setCurrentColor = (number) =>{
  activeColor.value = number
  setNewColor.value = true

}
const dragStartEvent = (event, item) =>{
  event.dataTransfer.dropEffect = 'move'
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('itemID',item)
}
</script>

<template>
  <div class="h-3/6  w-[300px] mt-12 mx-10 rounded-xl relative"
       :class="{'first: bg-[#BBCCDD]':activeColor === 1,
                'second: bg-[#8C7E90]':activeColor === 2,
                'third: bg-[#AB400C]':activeColor === 3,
                'fourth: bg-[#965B80]':activeColor === 4,
                'fifth: bg-[#C1AD43]':activeColor === 5}">

    <h1 class="px-[15px] pt-[30px] text-[22px] font-bold text-center roboto rounded-t-xl" :class="{
                'first: text-white':activeColor === 1,
                'second: text-white':activeColor === 2,
                'third: text-white':activeColor === 3,
                'fourth: text-white':activeColor === 4,
                'fifth: text-black':activeColor === 5}">
      {{props.value.Header}}</h1>
    <p class="">{{props.value.id}}</p>
    <div v-show="!setNewColor" class="w-[150px] h-[40px] absolute top-[-50px] right-0 flex justify-around items-center border-2 rounded-xl">
      <div class=""><button @click="setCurrentColor(1) "  class="bg-[#BBCCDD] w-[20px] mt-[5px] h-[20px]"></button></div>
      <div class=""><button @click="setCurrentColor(2) " class="bg-[#8C7E90] w-[20px] mt-[5px] h-[20px]"></button></div>
      <div class=""><button @click="setCurrentColor(3) " class="bg-[#AB400C] w-[20px] mt-[5px] h-[20px]"></button></div>
      <div class=""><button @click="setCurrentColor(4)" class="bg-[#965B80] w-[20px] mt-[5px] h-[20px]"></button></div>
      <div class=""><button @click="setCurrentColor(5) " class="bg-[#C1AD43] w-[20px] mt-[5px] h-[20px]"></button></div>
    </div>
    <div class="absolute top-[0px] right-[0px]">
      <button class="hover:bg-green-400 hover:duration-300 rounded h-[39px]" @click="setNewColor = !setNewColor" title="Change list color">
        <img src="../components/icons/settings-2-svgrepo-com.svg"  alt="" class="h-[30px] w-[35px] fill-amber-500 fill-white">
      </button>
      <button class="hover:bg-red-500 hover:duration-300 rounded-xl h-[40px]" @click="$emit('deleteList')" title="Delete a list">
        <img src="../components/icons/close-svgrepo-com.svg"  alt="" class="h-[30px] w-[35px]">
      </button>
    </div>
    <div class="absolute top-[0px] left-[0px] flex">
      <button @click="$emit('moveListToTheLeft')" class="" :class="{
                'first: pointer-events:none bg-red-300 cursor-auto':props.value.id === 1,
                }"><img src="../components/icons/chevron-left-svgrepo-com.svg" alt="" width="30" height="20"></button>
      <button @click="$emit('moveListToTheRight')" class="" :class="{
                'first: text-white':props.value.id === props,
                'second: text-white':activeColor === 2}"><img src="../components/icons/chevron-right-svgrepo-com.svg" alt="" width="30" height="20"></button>
    </div>
    <ul class="w-full p-[15px]"
        @drop="$emit('dragDropEvent',$event)"
        @dragover.prevent
        @dragenter.prevent>
      <li class="cursor-pointer w-full h-[60px] whitespace-nowrap overflow-hidden overflow-ellipsis cardo  text-[16px] bg-white rounded my-6 flex justify-center items-center p-2 font-normal" v-for="value in itemValue" :key="value.index"
      @dragstart="dragStartEvent($event, value.text)"
      draggable="true"
      @click="setCurrentTaskEl(value.Tasks)"
      >
        <p>
          {{value.text}}
        </p>
      </li>
    </ul>
    <div class="mb-8 flex flex-col justify-around items-center ">
      <div class="my-4" v-show="!setNewTask">
        <div class="flex">
          <input type="text" class="pl-2 w-full h-[35px] rounded outline-none" placeholder="Push 'Enter' to add task"  @keydown="$emit(modelValue,$event.target.value)" v-model="newTaskValue">
          <button class="text-white ml-4 font-bold" @click="setNewTask = !setNewTask" :class="{'first: text-white':activeColor === 1,
                'second: text-white':activeColor === 2,
                'third: text-white':activeColor === 3,
                'fourth: text-black':activeColor === 4,
                'fifth: text-black':activeColor === 5}">X</button>
        </div>
      </div>
      <button class="rounded-xl w-[50px] h-[40px] bg-white" v-show="setNewTask" @click="setNewTask = !setNewTask">+</button>
    </div>
  </div>
</template>

<style >
.roboto{
  font-family: 'Roboto', sans-serif;
}
.cardo{
  font-family: 'Cardo', serif;
}
</style>