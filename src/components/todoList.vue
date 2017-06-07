<template>
    <ul class="list-group">
        <li class="list-group-item" :class="{ 'completed' : todo.completed }" v-for="(todo,index) in todos">
        {{ todo.title }}
        <button class='btn btn-warning btn-xs pull-right' v-on:click='delTodo(index)'>delete</button>
        <button class='btn btn-xs pull-right margin-right-10' :class="[ todo.completed ? 'btn-danger' : 'btn-success' ]" v-on:click="toggleComplete(todo)">
        {{ todo.completed ? 'undo' : 'complete' }}
        </button>
        </li>
    </ul>
</template>
<script>
    export default{
        name:'todoList',
        props:['todos'],
        methods:{
          toggleComplete(todo){
              todo.completed = !todo.completed
          },
          delTodo(index){
              this.todos.splice(index,1);
          }
        }
    }
</script>
<style>
    .completed{
        color:#5cb85c;
        text-decoration: line-through;
    }
    .margin-right-10{
        margin-right: 10px;
    }
</style>
