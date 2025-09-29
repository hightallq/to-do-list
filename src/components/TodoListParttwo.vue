<script setup lang="ts">
// import Examples from './examples/Examples.vue';
import { ref, computed } from 'vue';

// give each todo a unique id
let id = 0

const newTodo = ref('')

const todos = ref([
    { id: id++, text: 'Learn HTML', completed: true },
    { id: id++, text: 'Learn JavaScript', completed: true },
    { id: id++, text: 'Learn Vue', completed: false },
    { id: id++, text: 'Learn CSS', completed: false },
    { id: id++, text: 'Learn NodeJS', completed: true },
    { id: id++, text: 'Learn ReactJS', completed: true }
])

// create new object to push, rewrite text to new variable 
function addTodo() {
    let newText = newTodo.value;
    let newTodoObject = { id: id++, text: newText, completed: false };
    todos.value.push(newTodoObject);
}

// remove todo by it's reference
function removeTodo(removeIndex) {
    todos.value = todos.value.filter((todo, index) => index != removeIndex);
}

let completed = ref(null);
let filteredTodos = computed(() => {
    return todos.value.filter((todo) => {
        return todo.completed == completed.value;
    })
})

// Sort func

// function sortTodos() {
//     function funcHelper(todo) {
//         return +todo.completed;
//     }
//     todos.value.sort(funcHelper);
// }

function sortTodos(todo, index) {
    todos.value.splice(index, 1)

    if (todo.completed) {
        todos.value.push(todo);
    }
    else {
        todos.value.unshift(todo)
    }
}

</script>

<template>
    <div class="lesson">
        <div class="todo-list">

            <form @submit.prevent="addTodo">
                <input v-model="newTodo">
                <button>Add Todo</button>
            </form>
            <ul>
                <li v-for="(todo, index) in (completed == null) ? todos : filteredTodos" :key="todo.id">
                    <input type="checkbox" v-model="todo.completed" @change="sortTodos(todo, index)">
                    <span :class="{ 'checked': todo.completed }"> {{ todo.text }} </span>
                    <button @click="removeTodo(index)">X</button>
                </li>
            </ul>
            <button class="show-all" @click="completed = null">Show all todos</button>
            <button class="show-checked" @click="completed = !completed">Show {{ completed ? "uncompleted" :
                "completed" }}</button>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.checked {
    text-decoration: line-through
}
</style>