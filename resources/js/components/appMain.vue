<template>
    <div class="todoListContainer">
        <div class="heading">
            <h2 id="title">Lista zadań</h2>
            <add-item-form
            v-on:itemchanged="getList()"/>
        </div>
    <list-view
    :items="items"
    v-on:reloadlist="getList()"/>
    </div>
</template>

<script>
import AddItemForm from './addItemForm.vue'
import ListView from './listView.vue'
    export default {
        components:{
            AddItemForm,
                ListView
        },
        data: function() {
            return {
                items: []
            }
        },
        methods: {
            getList() {
                axios.get('api/items')
                .then( response => {
                    this.items = response.data
                })
                .catch( error =>{
                    console.log(error);
                })
            }
        },
        created: function() {
            this.getList();
        }
    }
</script>

<style scoped>
.todoListContainer{
    width:500px;
    margin: auto;
}
.heading{
    background: #4977cc;
    padding:10px;
    font-family: 'Roboto','sans-serif';
    text-transform: uppercase;
}
#title {
    text-align: center;
    color: #fff;
    padding:10px;
}
</style>