<template>
    <div class='add'>
    <input type='text' class='form-control' placeholder='Add a new todo' v-model="newTodo.title">
    <button class='btn btn-success add_button' v-on:click="addNewTodo(newTodo)">Add Todo</button>
    </div>
</template>
<script>
    export default{
        props:['todos'],
        data(){
            return{
                newTodo : {'id':null,title:'',completed:0}
            }
        },
        methods:{
          addNewTodo(newTodo){
              if(newTodo.title){
                  this.todos.push(newTodo)
                  this.newTodo = {id:null,title:'',completed:0}
              }
          }
        }
    }
</script>

<style>
    .add_button{margin-top: 20px}
</style>
