<script setup>
import {ref,computed} from "vue";
import ToDoList from '../components/ToDoList.vue'

const addNewTask = ref('')
const onInput = (e) =>{
  addNewTask.value = e.target.value
  if(e.code === 'Enter') {
    dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.push(e.target.value)

  }
}

const deleteList = () => {
  /*toDoLists.splice(Number(localStorage.getItem('currentTaskBoardId')),1)*/
  if(Number(localStorage.getItem('currentTaskBoardId')-1) === 0){
    dynamicRenderingLists.value.shift()
    console.log('Impossible to cut meat 1111')
    for(let i = 0;i<dynamicRenderingLists.value.length;i++){
      dynamicRenderingLists.value[i].id = dynamicRenderingLists.value[i].id-1
    }
  }
  else{
    if((Number(localStorage.getItem('currentTaskBoardId')-1)===dynamicRenderingLists.value.length)){
        dynamicRenderingLists.value.pop()
    }
    else{
      for(let i = Number(localStorage.getItem('currentTaskBoardId'));i<dynamicRenderingLists.value.length;i++){
        dynamicRenderingLists.value[i].id = dynamicRenderingLists.value[i].id-1
      }
      dynamicRenderingLists.value.splice(Number(localStorage.getItem('currentTaskBoardId')-1),1)
    }
  }

  console.log(dynamicRenderingLists.value.length)
  console.log(dynamicRenderingLists.value)
    console.log(toDoLists)
}

const createList = () =>{
  if(toDoLists.length>0) {
    toDoLists.splice(
        dynamicRenderingLists.value.length, 1, {id:dynamicRenderingLists.value.length + 1, Tasks:[], Header:'Hello, we need to go!'})
    console.log(dynamicRenderingLists.value.length)
  }
  else{
    toDoLists.splice(
        dynamicRenderingLists.value.length, 1, {id:dynamicRenderingLists.value.length + 1, Tasks:[], Header:'Hello, we need to go!'})
    console.log(dynamicRenderingLists.value.length)
  }
}

const toDoLists = [
    {
      id:1,
      Header:'Things to do',
      Tasks:['Task 1', 'Task 2'],
    },
  {
    id:2,
    Header:'Things to do',
    Tasks:['Task 3', 'Task 4']
  },
  {
    id:3,
    Header:'Things to do',
    Tasks:['Task 5', 'Task 6']
  },
  {
    id:4,
    Header:'Things to do',
    Tasks:['Task 7', 'Task 8']
  },
  {
    id:5,
    Header:'Things to do',
    Tasks:['Task 9', 'Task 10']
  },
  {
    id:6,
    Header:'Things to do',
    Tasks:['Task 11', 'Task 12']
  },

]
const dynamicRenderingLists = computed(() => toDoLists)

const setCurrentTaskBoardId = (id) => {
  if(localStorage.getItem('currentTaskBoardId') == id) {
      console.log('This id currently used')
  }
  else{
    localStorage.setItem('currentTaskBoardId',id.toString())
    console.log(localStorage.getItem('currentTaskBoardId'))
  }
}


</script>

<template>
  <button class="" @click="createList">e2w</button>
  <div class="h-auto mb-48 mt-10">
    <div class=" flex flex-wrap">
        <ToDoList v-for="item in dynamicRenderingLists" @deleteList="deleteList" v-model="addNewTask" @mousemove="onInput" @keydown="onInput" :key="item.id" :item-value="item.Tasks" :value="item" @click="setCurrentTaskBoardId(item.id)" @setNewTask=''></ToDoList>
    </div>
  </div>
</template>

<style scoped>

</style>