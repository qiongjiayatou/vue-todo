<template>
    <div>

        <add-item @add:item="addItem"></add-item>

        
        <p v-if="todoItems.length == 0">No tasks yet! Go ahead, create one!</p>

        <table v-else class="table">
        <thead>
            <tr>
                <!-- <th>#</th> -->
                <th style="width: 20%">Completed</th>
                <th>Title</th>
                <th></th>
            </tr>
            
            
        </thead>
        <tbody>
            <!-- <todo-item v-for="item in todoItems" :title="item.title" :completed="item.completed" :key="item.id"></todo-item> -->
            <tr v-for="item in todoItems" :key="item.id">    
                <td>
                    <span v-if="item.completed"
                        @click="markItem(item.id)" class="icon">✔️</span>
                    <span v-else
                        @click="markItem(item.id)" class="icon">🔲</span>
                </td>   
                
                <td v-if="editing === item.id">
                    <div class="form-group">
                        <input type="text" class="form-control" v-model="item.title">
                    </div>
                </td>

                <td v-else v-text="item.title"></td>   
                
                <td v-if="editing === item.id">
                    <button @click="saveEditing(item)" class="btn btn-primary mr-3">Save</button>
                    <button @click="cancelEditing(item)" class="btn btn-secondary">Cancel</button>
                </td>
                <td v-else>
                    <button @click="editMode(item)" class="btn btn-warning mr-3">Edit</button> 
                    <button @click="deleteItem(item.id)" class="btn btn-danger">Delete</button>
                </td>
            </tr>
        </tbody>
        </table>
    </div>    
</template>

<script>
import axios from 'axios'
import AddItem from './AddItem.vue'

export default {

    name: 'todo-list',
    components: {
        AddItem
    },

    data() {
        return {
            editing: null,
            cachedItem: null,
            todoItems: [
                {
                    id: 1,
                    title: "Shopping",
                    completed: false
                },
                {
                    id: 2,
                    title: "Check an email",
                    completed: true
                },
                {
                    id: 3,
                    title: "Walk a dog",
                    completed: false
                },
            ]
        }
    },
    methods: {
        getTodos() {
            axios.get('https://jsonplaceholder.typicode.com/todos')
                .then(response => {
                    console.log(response.data);
                    this.todoItems = response.data;
                })
                .catch(error => console.log(error));

        },
        markItem(id) {

            this.todoItems.map((item) => {
                if (item.id === id) {
                    item.completed = ! item.completed;
                }
                
            });
        },
        addItem(newTitle) {
            const newId = this.todoItems[this.todoItems.length - 1].id + 1;

            const newItem = {
                id: newId,
                title: newTitle,
                completed: false,
            }

            this.todoItems = [...this.todoItems, newItem];
        },
        editMode(item) {
            this.cachedItem = Object.assign({}, item);
            this.editing = item.id;

        },
        cancelEditing(item) {
            Object.assign(item, this.cachedItem);
            this.editing = null;
        },
        saveEditing(item) {
            console.log(item);
            this.editing = null;
            // put request
        },
        deleteItem(id) {
            this.todoItems = this.todoItems.filter((item) => item.id !== id)
        }
    },
    mounted() {
        // this.getTodos();
    }

    
}
</script>

<style scoped>
    .icon {
        cursor: pointer;
    }

</style>