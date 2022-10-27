<script setup>
import Header from '../components/header.vue'
import { auth } from '../firebase'
import { signInWithEmailAndPassword, GoogleAuthProvider, signInWithPopup, onAuthStateChanged } from "firebase/auth";
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


function signUpWithGoogle() {
    const provider = new GoogleAuthProvider();
    signInWithPopup(auth, provider)
        .then((result) => {
            // This gives you a Google Access Token. You can use it to access the Google API.
            const credential = GoogleAuthProvider.credentialFromResult(result);
            const token = credential.accessToken;
            // The signed-in user info.
            const user = result.user;
            // ...
            router.push('/')
        }).catch((error) => {
            // Handle Errors here.
            const errorCode = error.code;
            const errorMessage = error.message;
            // The email of the user's account used.
            const email = error.email;
            // The AuthCredential type that was used.
            const credential = GoogleAuthProvider.credentialFromError(error);
            // ...
        });
}


onMounted(() => {
    onAuthStateChanged(auth, (user) => {
        if (user) {
            // User is signed in, see docs for a list of available properties
            // https://firebase.google.com/docs/reference/js/firebase.User
            router.push('/')
            // ...
        } else {
            // User is signed out
            // ...

        }
    });
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
            <button class="p-2 rounded-sm bg-white text-black" @click="signUpWithGoogle">Log in with
                Google</button>

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