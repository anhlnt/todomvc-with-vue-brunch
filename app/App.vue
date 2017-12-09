<template>
  <div id="app">
    <my-header :todos="todos"></my-header>
    <my-main :todos="todos" :editedTodo="editedTodo" :visibility="visibility"></my-main>
    <my-footer :todos="todos" :visibility="visibility" v-on:removeCompleted="removeCompleted" v-on:toAll="toAll" v-on:toActive="toActive" v-on:toCompleted="toCompleted"></my-footer>
  </div>
</template>

<script>
import Vue from 'vue'
import Header from './components/Header'
import Main from './components/Main'
import Footer from './components/Footer'

Vue.component('my-header', Header);
Vue.component('my-main', Main);
Vue.component('my-footer', Footer);

var STORAGE_KEY = 'todos-vuejs';

var todoStorage = {
    fetch: function () {
        var todos = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
        todos.forEach(function (todo, index) {
            todo.id = index
        })
        todoStorage.uid = todos.length
        return todos
    },
    save: function (todos) {
        localStorage.setItem(STORAGE_KEY, window.JSON.stringify(todos));
    }
};
var filters = {
all: function (todos) {
    return todos;
},
active: function (todos) {
    return todos.filter(function (todo) {
        return !todo.completed;
    });
},
completed: function (todos) {
    return todos.filter(function (todo) {
        return todo.completed;
    });
}
};
export default {
name: 'app',
data() {
    return {
        todos: todoStorage.fetch(),
        newTodo: '',
        editedTodo: null,
        visibility: 'all',
    }
},
watch: {
		todos: {
			deep: true,
			handler: todoStorage.save
		}
},
methods: {
removeCompleted: function () {
    this.todos = filters.active(this.todos);
    },
toAll: function(){
    this.visibility = 'all'
},
toActive: function(){
    this.visibility = 'active'
},
toCompleted: function(){
    this.visibility = 'completed'
}

}
}

</script>

<style>
#app {
	background: #fff;
	margin: 130px 0 40px 0;
	position: relative;
	box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2),
	            0 25px 50px 0 rgba(0, 0, 0, 0.1);
}
</style>
