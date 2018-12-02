<template>
    <div>
        
        <div class="d-flex justify-content-center card-body" v-if="editingItem !== item">
            <input type="checkbox" v-model="item.finished">
        <div>{{ item.id }} . {{ item.title }}</div>

            <button @click="editTask()" class="btn-warning ml-2">Edit</button>
            <button @click="deleteTask()" class="btn-danger ml-1" >Delete</button>
            <i class="fas fa-star star" :class="{ 'active': item.important }" @click="makeImp()"></i> 
        </div>
        <task-form v-else
         @submit="updateTask"
         @cancel="cancelEdit"
         :item="item"
        >
        </task-form>
    </div>

</template>
<script>
import taskForm from "./form.vue"

export default {
    components: {
        taskForm
    },
    props: ['item','editingItem','updateTask','cancelEdit'],
    methods: {
        editTask() {
            this.$emit('editTask',this.item) 
        },
        deleteTask() {
            this.$emit('deleteTask',this.item)
        },
        makeImp() {
            this.$emit('makeImp',this.item)
        }
        
    }
}
</script>

<style>
.star.active {
    color: yellow;
}
</style>
