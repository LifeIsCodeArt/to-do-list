<script setup>
import {ref,computed,provide} from "vue";
import ToDoList from '../components/ToDoList.vue'

const addNewTask = ref('')

const onInput = (e) =>{
  addNewTask.value = e.target.value
  if(e.code === 'Enter') {
    dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.push(
        {
          id:dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.length+1,
          text:`${e.target.value}`
        }
    )

  }
}

const deleteList = () => {
  if(Number(localStorage.getItem('currentTaskBoardId')-1) === 0){
    dynamicRenderingLists.value.shift()
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
  if(toDoLists.value.length>1) {
    toDoLists.value.splice(
        dynamicRenderingLists.value.length,
        1,
        {id:dynamicRenderingLists.value.length + 1,
               Tasks:[],
               Header:'Hello, we need to go!'}
            )
  }
  else{
    toDoLists.value.push(
        {
          id:1,
          Tasks:[],
          Header:'Hello, we need to go!'
        }
    )
  }
}

const toDoLists = ref([
    {
      id:1,
      Header:'Things to do 1',
      Tasks:[{id:1, text:'Task 1'},
            ]
    },
  {
    id:2,
    Header:'Things to do 2',
    Tasks:[{id:1, text:'Task 3'},
           {id:2, text:'Task 4'}
          ]
  },
  {
    id:3,
    Header:'Things to do 3',
    Tasks:[
        {id:1, text:'Task 5'},
        {id:2, text:'Task 6'}
          ]
  },
  {
    id:4,
    Header:'Things to do 4',
    Tasks:[
        {id:1, text:'Task 7'},
        {id:2, text:'Task 8'}
          ]
  },
  {
    id:5,
    Header:'Things to do 5',
    Tasks:[
        {id:1, text:'Task 9'},
        {id:2, text:'Task 10'}
          ]
  },
  {
    id:6,
    Header:'Things to do 6',
    Tasks:[
        {id:1, text:'Task 11'},
        {id:2, text:'Task 12'}
          ]
  },
])
const dynamicRenderingLists = computed(() => toDoLists.value)

const setCurrentTaskBoardId = (id) => {
  if(localStorage.getItem('currentTaskBoardId') == id) {
      console.log('This id currently used')
  }
  else{
    localStorage.setItem('currentTaskBoardId',id.toString())
  }
}

const dragDropEvent = (event, id) =>{
  const itemID = event.dataTransfer.getData('itemID')
  const actualEl2 =  dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.findIndex(
      el => el.text === event.dataTransfer.getData('itemID')
  )
  dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.splice(actualEl2,1)
  localStorage.setItem('currentTaskBoardId', id)
  dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.push(
      {id:dynamicRenderingLists.value[localStorage.getItem('currentTaskBoardId')-1].Tasks.length+1, text:itemID}
  )
}


const moveListToTheLeft = (id) => {
  [dynamicRenderingLists.value[id-2], dynamicRenderingLists.value[id-1]]  = [dynamicRenderingLists.value[id-1], dynamicRenderingLists.value[id-2]]
  const holder = dynamicRenderingLists.value[id-2].id
  dynamicRenderingLists.value[id-2].id = dynamicRenderingLists.value[id-1].id
  dynamicRenderingLists.value[id-1].id = holder
}
const moveListToTheRight = (id) =>{
  [dynamicRenderingLists.value[id-1], dynamicRenderingLists.value[id]]  = [dynamicRenderingLists.value[id], dynamicRenderingLists.value[id-1]]
  const holder = dynamicRenderingLists.value[id-1].id
  dynamicRenderingLists.value[id-1].id = dynamicRenderingLists.value[id].id
  dynamicRenderingLists.value[id].id = holder
}

</script>

<template>
  <button class="" @click="createList">+</button>
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
                  @dragDropEvent="dragDropEvent($event, item.id)"
                  @moveListToTheLeft="moveListToTheLeft(item.id)"
                  @moveListToTheRight="moveListToTheRight(item.id)"></ToDoList>
    </div>
  </div>
</template>

<style scoped>

</style>