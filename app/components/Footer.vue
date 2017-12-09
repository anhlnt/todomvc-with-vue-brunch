<template>
    <footer class="footer" v-show="todos.length">
        <span class="todo-count">
            <strong v-text="remaining"></strong> {{pluralize('item', remaining)}} left
        </span>
        <ul class="filters">
            <li @click="toAll"><a href="#/all" :class="{selected: visibility == 'all'}" >All</a></li>
            <li @click="toActive"><a href="#/active" :class="{selected: visibility == 'active'}" >Active</a></li>
            <li @click="toCompleted"><a href="#/completed" :class="{selected: visibility == 'completed'}" >Completed</a></li>
        </ul>
        <button class="clear-completed" v-on:click="removeCompleted" v-show="todos.length > remaining">
            Clear completed
        </button>
    </footer>
</template>

<script>
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
    name: 'footer',
    props: {
        todos: Array,
        visibility: String,
    },
    methods: { 
        pluralize: function (word, count) {
				return word + (count === 1 ? '' : 's');
		},
        removeCompleted: function () {
            this.$emit('removeCompleted')
        },
        toAll: function(){
            this.$emit('toAll')
        },
        toActive: function(){
            this.$emit('toActive')
        },
        toCompleted: function(){
            this.$emit('toCompleted')
        }
    },
    computed: {
        remaining: function () {
            return filters.active(this.todos).length;
        }
    },
}

</script>

<style>
.footer {
	color: #777;
	padding: 10px 15px;
	height: 20px;
	text-align: center;
	border-top: 1px solid #e6e6e6;
}

.footer:before {
	content: '';
	position: absolute;
	right: 0;
	bottom: 0;
	left: 0;
	height: 50px;
	overflow: hidden;
	box-shadow: 0 1px 1px rgba(0, 0, 0, 0.2),
	            0 8px 0 -3px #f6f6f6,
	            0 9px 1px -3px rgba(0, 0, 0, 0.2),
	            0 16px 0 -6px #f6f6f6,
	            0 17px 2px -6px rgba(0, 0, 0, 0.2);
}
.todo-count {
	float: left;
	text-align: left;
}

.todo-count strong {
	font-weight: 300;
}
.filters {
	margin: 0;
	padding: 0;
	list-style: none;
	position: absolute;
	right: 0;
	left: 0;
}

.filters li {
	display: inline;
}

.filters li a {
	color: inherit;
	margin: 3px;
	padding: 3px 7px;
	text-decoration: none;
	border: 1px solid transparent;
	border-radius: 3px;
}

.filters li a.selected,
.filters li a:hover {
	border-color: rgba(175, 47, 47, 0.1);
}

.filters li a.selected {
	border-color: rgba(175, 47, 47, 0.2);
}
.clear-completed,
html .clear-completed:active {
	float: right;
	position: relative;
	line-height: 20px;
	text-decoration: none;
	cursor: pointer;
	position: relative;
}

.clear-completed:hover {
	text-decoration: underline;
}
</style>


