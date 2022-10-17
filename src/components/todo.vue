<script>
export default {
    name: 'Todo',
    data() {
        return {
            todos: [],
        }
    },
    methods: {
        addTodo() {
            if (this.todo != "") {
                this.todos.push({
                    id: this.todos.length + 1,
                    title: this.todo,
                    completed: false,
                    created: new Date().toLocaleString().replace(/(.*)\D\d+/, '$1')
                });
                this.todo = "";
            }

        },
        removeTodo(id) {
            this.todos = this.todos.filter(todo => todo.id !== id);
        },
        doneTodo(id) {
            this.todos = this.todos.map(todo => {
                if (todo.id === id) {
                    todo.completed = !todo.completed;
                }
                return todo;
            });
        }

    },
}
</script>

<template>
    <div class="flex justify-center w-full">
        <div id="todo-div" class="flex items-center justify-center p-5 w-2/3">
            <input id="todo-input" class="text-black w-3/5 p-2 rounded-md" type="text" v-model="todo"
                placeholder="What to do..." />
            <button id="add-todo-button" class="bg-gray-500 w-1/5 ml-2 p-2" @click="addTodo">Add</button>
        </div>
    </div>

    <div class="flex justify-center m-auto w-full">
        <ul class="w-4/5">
            <li id="todo-item" class="flex justify-between items-center bg-slate-400 bg-opacity-30 p-3 mb-3 rounded-md"
                :class="{ 'bg-green-800 bg-opacity-40': todo.completed }" v-for="todo in todos" :key="todo.id"
                :id="todo.id">
                <div>
                    <p class="text-lg" :class="[todo.completed === true ? 'line-through' : '']">
                        {{todo.title}}
                    </p>
                    <p class="text-xs text-slate-500">{{todo.created}}</p>
                </div>
                <div class="flex no-wrap">
                    <button class="bg-green-800 p-2 h-fit mr-2" @click="doneTodo(todo.id)">&check;</button>
                    <button class="bg-red-800 p-2 h-fit" @click="removeTodo(todo.id)">&times;</button>
                </div>
            </li>
        </ul>
    </div>
</template>

<style scoped>
#add-todo-button:hover {
    background-color: #4CAF50;
}

#todo-item {
    box-shadow: -2px 3px 9px -3px rgba(196, 196, 196, 0.36);
    -webkit-box-shadow: -2px 3px 9px -3px rgba(196, 196, 196, 0.36);
    -moz-box-shadow: -2px 3px 9px -3px rgba(196, 196, 196, 0.36);
}

#todo-item:hover {
    transform: scale(1.02);
}

@media only screen and (max-width: 1200px) {

    /*Tablets [601px -> 1200px]*/
    #todo-div {
        width: 70%;
    }

    #todo-input {
        /*width: 100%;*/
        width: 100%;
    }
}

@media only screen and (max-width: 600px) {

    /*Big smartphones [426px -> 600px]*/
    #todo-div {
        width: 90%;
    }

    #todo-input {
        /*width: 100%;*/
        width: 100%;
    }

    #add-todo-button {
        width: 20%;
    }
}

@media only screen and (max-width: 425px) {

    /*Small smartphones [325px -> 425px]*/
    #todo-div {
        width: 90%;
    }

    #todo-input {
        /*width: 100%;*/
        width: 100%;
    }
}
</style>