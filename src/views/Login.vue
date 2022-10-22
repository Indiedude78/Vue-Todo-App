<script setup>
import Header from '../components/header.vue'
import { auth } from '../firebase'
import { signInWithEmailAndPassword } from "firebase/auth";
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'

let emailInput = ref('')
let passInput = ref('')
const router = useRouter()

async function login() {
    let email = emailInput.value
    let pass = passInput.value
    signInWithEmailAndPassword(auth, email, pass)
        .then((userCredential) => {
            // Signed in
            const user = auth.currentUser;
            // ...
            router.push('/')
        })
        .catch((error) => {
            const errorCode = error.code;
            const errorMessage = error.message;
        });
}

onMounted(() => {
    if (auth.currentUser) {
        router.push('/')
    }
});
</script>

<template>
    <Header title="Login" />

    <div class="flex justify-center">
        <form class="flex flex-col align-middle w-2/5" v-on:submit.prevent="">
            <label class="p-1" for="email">Email</label>
            <input class="py-1 px-2 rounded-sm text-black" type="email" id="email" placeholder="Email"
                v-model="emailInput" />
            <label class="p-1" for="password">Password</label>
            <input class="py-1 px-2 rounded-sm text-black" type="password" id="password" placeholder="Password"
                v-model="passInput" />
            <button class="my-3 p-2 rounded-sm bg-blue-500 text-white hover:bg-white hover:text-blue-500" type="submit"
                @click="login">Log in</button>

        </form>
    </div>
</template>

<style scoped>
@media only screen and (max-width: 1200px) {

    /*Tablets [601px -> 1200px]*/
    form {
        width: 40%;
    }
}

@media only screen and (max-width: 650px) {

    /*Big smartphones [426px -> 600px]*/

    form {
        width: 85%;
    }


}

@media only screen and (max-width: 525px) {

    /*Small smartphones [325px -> 425px]*/

    form {
        width: 90%;
    }
}
</style>