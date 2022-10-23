<script setup>
import Todo from '../components/todo.vue'
import Header from '../components/header.vue'
import { auth } from '../firebase'
import { onAuthStateChanged } from "firebase/auth";
import { ref } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const loggedUser = ref(null)

onAuthStateChanged(auth, (user) => {
    if (user) {
        // User is signed in, see docs for a list of available properties
        // https://firebase.google.com/docs/reference/js/firebase.User
        const uid = user.uid;
        loggedUser.value = user
        // ...
    } else {
        // User is signed out
        // ...
        router.push('/login')

    }
});

</script>

<template>
    <Header title="Gotta Do!" />
    <Todo />

</template>