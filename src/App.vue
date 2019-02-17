<template>
  <div id="app">
    <button @click="setFilter('all')">All</button>
    <button @click="setFilter('inProgress')">in Progress</button>
    <button @click="setFilter('finished')">Finished</button>
    
    <div class="mt-4">
      
      <task-form @submit="addTask" @cancel="clearInput"></task-form>
    </div>
    <draggable :filtered="filtered">
      <task-item class="items mt-4"
      v-for="item in filtered"
      v-bind:key="item.id"
      :editingItem="editingItem"
      :item="item"
      @editTask="editTask"
      @deleteTask="deleteTask"
      @makeImp="makeImp"
      :updateTask="updateTask"
      :cancelEdit="cancelEdit"
      
      ></task-item>
    </draggable>
    <!-- <div class="items mt-4" v-for="item in filtered" v-bind:key="item.id" >
        <div class="d-flex justify-content-center card-body" v-if="editingItem !== item">
          <input type="checkbox" v-model="item.finished">
          <div>{{ item.id }} . {{ item.title }}</div>
           <button @click="editTask(item)" class="btn-warning">Edit</button>
           <button @click="deleteTask(item)" class="btn-alert" >Delete</button>
          
        </div>
        <div v-else>
           <input type="text" v-model="item.title">
            <button @click="updateTask(item)">Submit</button>
            <button @click="cancelEdit()" class="btn-primary" >Cancel</button>
        </div>
    </div> -->

  </div>
</template>

<script>
import taskForm from "./components/form.vue"
import taskItem from "./components/taskItem.vue" //命名時注意規則，不要和html的重複
import draggable from 'vuedraggable'

export default {
  components:{
    taskForm,taskItem,draggable
  },
  name: 'App',
  data() {
    return {
      items: [],
      newTaskTitle:'',
      editingItem:{},
      filterName:'all',
      myArray:[]
    }
  },
  computed: {
    filtered() {
      let items = []
      if(this.filterName =='inProgress'){
        for(let i=0;i<this.items.length;i++){
            if(!this.items[i].finished){
              items.push(this.items[i])
            }
        }
        return items
      }else if(this.filterName =='finished'){
        for(let i=0;i<this.items.length;i++){
          if(this.items[i].finished === true){
            items.push(this.items[i])
          }
        }
        return items
      }else{
        return this.items
      }
      
    }
  },
  methods: {
    addTask(form) {
        let newItem = {
          id : Date.now(),
          title : form.title,
          important: false,
          finished: false
        }
        this.items.unshift(newItem)
        form.title = ''
    },
    editTask(item) {
        this.editingItem = item
    },
    updateTask(form) {
        form.title = this.editingItem.title 
        this.editingItem = null
    },
    cancelEdit() {
        this.editingItem = null
    },
    deleteTask(item) {
        let index = this.items.indexOf(item)
        this.items.splice(index,1)
    },
    setFilter(data) {
      this.filterName = data
    },
    clearInput() {
      this.newTaskTitle = ''
    },
    makeImp(item) {
      item.important = !item.important
      let index = this.items.indexOf(item)
      this.items.splice(index,1)
      this.items.unshift(item)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container{
  max-width: 680px;
}
.card input{
  width: 100%;  
}
.star{
  cursor: pointer;
}
</style>
