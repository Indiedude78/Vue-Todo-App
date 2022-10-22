<script setup>
import { auth } from '../firebase'
import { onAuthStateChanged } from "firebase/auth";
import { ref, onMounted } from 'vue'


let loggedUser = ref(null)
onAuthStateChanged(auth, (user) => {
    if (user) {
        loggedUser.value = user
    } else {
        loggedUser.value = null
    }
});


</script>

<template>
    <div class="w-full border-b-2" id="navigation-div">
        <nav class="flex flex-row align-center justify-between p-2">
            <div class="m-1">
                <router-link class="p-2 mx-1 text-lg" to="/">Home</router-link>
                <router-link v-if="loggedUser" class="p-2 mx-1 text-lg" to="/logout">Logout</router-link>
            </div>
            <div class="m-1">
                <router-link v-if="!loggedUser" class="p-2 mx-1 text-lg" to="/login">Login</router-link>
                <router-link v-if="!loggedUser"
                    class="p-2 mx-1 text-lg bg-blue-500 hover:bg-white hover:text-blue-500 rounded-sm" to="/register">
                    Register</router-link>
            </div>
        </nav>
    </div>
</template>


