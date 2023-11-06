<script setup>
import {ref,computed} from "vue";
import ToDoList from '../components/ToDoList.vue'

const addNewTask = ref('')
const onInput = (e) =>{
  addNewTask.value = e.target.value
  if(e.code === 'Enter') {
    console.log(dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks)
    dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.push({id:dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.length+1, text:`${e.target.value}`})

  }
}

const deleteList = () => {
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
}

const createList = () =>{
  if(toDoLists.length>0) {
    toDoLists.splice(
        dynamicRenderingLists.value.length, 1, {id:dynamicRenderingLists.value.length + 1, Tasks:[], Header:'Hello, we need to go!'})
    console.log(dynamicRenderingLists.value.length)
  }
}

const toDoLists = [
    {
      id:1,
      Header:'Things to do',
      Tasks:[{id:1, text:'Task 1'}, {id:2, text:'Task 2'}],
    },
  {
    id:2,
    Header:'Things to do',
    Tasks:[{id:1, text:'Task 1'}, {id:2, text:'Task 2'}]
  },
  {
    id:3,
    Header:'Things to do',
    Tasks:[{id:1, text:'Task 1'}, {id:2, text:'Task 2'}]
  },
  {
    id:4,
    Header:'Things to do',
    Tasks:[{id:1, text:'Task 1'}, {id:2, text:'Task 2'}]
  },
  {
    id:5,
    Header:'Things to do',
    Tasks:[{id:1, text:'Task 1'}, {id:2, text:'Task 2'}]
  },
  {
    id:6,
    Header:'Things to do',
    Tasks:[{id:1, text:'Task 1'}, {id:2, text:'Task 2'}]
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

const dragDropEvent = (event) =>{
  const itemID = event.dataTransfer.getData('itemID')
  dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.push({id:dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.length+1, text:itemID})
 const actualEl =  dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')].Tasks.filter(el => el.text === event.dataTransfer.getData('itemID'))
  console.log(Number(actualEl[0].id-1))
  dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')].Tasks.splice(Number(actualEl[0].id),1)
  console.log(dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')].Tasks.splice(actualEl[0].id,1))
}

</script>

<template>
  <button class="" @click="createList">e2w</button>
  <div class="h-auto mb-48 mt-10">
    <div class=" flex flex-wrap">
        <ToDoList v-for="item in dynamicRenderingLists"
                  @deleteList="deleteList"
                  v-model="addNewTask"
                  @mousemove="onInput"
                  @keydown="onInput"
                  :key="item.id"
                  :item-value="item.Tasks"
                  :value="item"
                  @mouseenter="setCurrentTaskBoardId(item.id)"
                  @dragDropEvent="dragDropEvent($event)"></ToDoList>
    </div>
  </div>
</template>

<style scoped>

</style>