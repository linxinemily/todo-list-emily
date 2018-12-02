<template>
    <div class="card">
        <div class="card-body">
            <input type="text" v-model="form.title">
        </div>
        <div class="card-header">
            <button @click="submit()">Submit</button>
            <button @click="cancel()">Cancel</button>
        </div>
    </div>
</template>

<script>
export default {
    props:[ 'item' ],
    data(){
        return{
            form: {
                title:''
            }
        }
    },
    mounted () {
        if (this.item) {
            this.form.title = this.item.title
        }
    },
    methods:{
        submit() {
            this.$emit('submit', this.form)
        },
        cancel() {
            this.$emit('cancel')
        }
    }

}
</script>
