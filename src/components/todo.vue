<script setup>
import { onMounted, ref } from 'vue'
import { onSnapshot, collection, addDoc, doc, updateDoc, deleteDoc, where, query } from "firebase/firestore";
import { db, auth } from '../firebase';
import { onAuthStateChanged } from "firebase/auth";

let todos = ref([]);
let todoInput = ref('');

let uid = ref(null);

onAuthStateChanged(auth, (user) => {
    if (user) {
        uid = user.uid;
        console.log(uid);
        const q = query(collection(db, "todos"), where("createdBy", "==", uid));
        onSnapshot(q, (querySnapshot) => {
            let fbTodos = [];
            querySnapshot.forEach((doc) => {
                let todo = doc.data().todo;
                let id = doc.id;
                let createdBy = doc.data().createdBy;
                let time = doc.data().time;
                let done = doc.data().done;
                fbTodos.push({ todo, id, createdBy, time, done });
            });
            todos.value = fbTodos;
            console.log(todos.value)
        });

    }
});

onMounted(async () => {

});

async function addTodo() {
    let currentTime = new Date().toLocaleString().replace(/(.*)\D\d+/, '$1');
    const docRef = await addDoc(collection(db, "todos"), {
        todo: todoInput.value,
        done: false,
        time: currentTime,
        createdBy: uid
    });
    console.log("Document written with ID: ", docRef.id);
    todoInput.value = "";
}

async function doneTodo(id) {
    todos.value.forEach(async (todo) => {
        if (todo.id === id) {
            todo.done = !todo.done;
            await updateDoc(doc(db, "todos", id), {
                done: todo.done
            });
        }
    });
}

async function removeTodo(id) {
    await deleteDoc(doc(db, "todos", id));
}



</script>

<template>
    <div class="flex justify-center w-full">
        <div id="todo-div" class="flex items-center justify-center p-5 w-4/5">
            <form class="flex w-2/3" v-on:submit.prevent="addTodo">
                <input id="todo-input" class="text-black w-4/5 p-2 rounded-md" type="text" v-model="todoInput"
                    placeholder="What to do..." />
                <button id="add-todo-button" class="bg-gray-500 w-1/5 ml-2 p-2">Add</button>
            </form>
        </div>
    </div>

    <div class="flex justify-center m-auto w-full">
        <ul class="w-4/5">
            <li id="todo-item" class="flex justify-between items-center bg-slate-400 bg-opacity-30 p-3 mb-3 rounded-md"
                :class="{ 'bg-green-800 bg-opacity-40': todo.done }" v-for="todo in todos" :key="todo.id" :id="todo.id">
                <div>
                    <p class="text-lg" :class="[todo.done === true ? 'line-through' : '']">
                        {{todo.todo}}
                    </p>
                    <p class="text-xs text-slate-500">{{todo.time}}</p>
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
        width: 100%;
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

    form {
        width: 90;
    }

    #add-todo-button {
        width: 20%;
    }
}

@media only screen and (max-width: 525px) {

    /*Small smartphones [325px -> 425px]*/
    #todo-div {
        width: 100%;
    }

    #todo-input {
        /*width: 100%;*/
        width: 100%;
    }

    form {
        width: 95%;
    }
}
</style>