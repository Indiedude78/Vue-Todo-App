<script setup>
import Header from '../components/header.vue'
import { ref, onMounted } from 'vue'
import { auth } from '../firebase'
import { createUserWithEmailAndPassword, GoogleAuthProvider, signInWithPopup } from "firebase/auth";
import { useRouter } from 'vue-router'

let emailInput = ref('')
let passInput = ref('')
let confirmPassInput = ref('')
let returnMsg = ref('')
const router = useRouter()

onMounted(() => {
    if (auth.currentUser) {
        router.push('/')
    }
});

//function to check if email is valid
function validateEmail(email) {
    const re = /\S+@\S+\.\S+/;
    return re.test(email);
}

function validatePassLength(pass) {
    if (pass.length < 8) {
        return false
    } else {
        return true
    }
}

function validatePassMatch(pass, confirmPass) {
    if (pass === confirmPass && pass != "" && confirmPass != "") {
        return true
    } else {
        return false
    }
}

function register() {
    let email = emailInput.value
    let pass = passInput.value
    let confirmPass = confirmPassInput.value
    if (validateEmail(email) && validatePassLength(pass) && validatePassMatch(pass, confirmPass)) {
        createUserWithEmailAndPassword(auth, email, pass)
            .then((userCredential) => {
                emailInput.value = ""
                passInput.value = ""
                confirmPassInput.value = ""
                // Signed in 
                const user = auth.currentUser;
                // ...
                router.push('/')
            })
            .catch((error) => {
                const errorCode = error.code
                const errorMessage = error.message
            });
    } else {
        returnMsg.value = "Please enter a valid email and password"
    }
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

</script>

<template>
    <Header title="Register" />
    <div class="flex justify-center">
        <form class="flex flex-col align-middle w-2/5" v-on:submit.prevent="">
            <label class="p-1" for="email">Email</label>
            <input class="py-1 px-2 rounded-sm text-black" type="email" id="email" placeholder="Email"
                v-model="emailInput" v-on:change="validateEmail(emailInput)" />
            <label class="p-1" for="password">Password</label>
            <input class="py-1 px-2 rounded-sm text-black" type="password" id="password" placeholder="Password"
                v-model="passInput" v-on:change="validatePassLength(passInput)" />
            <label class="p-1" for="confirm_password">Confirm Password</label>
            <input class="py-1 px-2 rounded-sm text-black" type="password" id="confirm_password"
                placeholder="Confirm Password" v-model="confirmPassInput"
                v-on:change="validatePassMatch(passInput, confirmPassInput)" />
            <button class="my-3 p-2 rounded-sm bg-blue-500 text-white hover:bg-white hover:text-blue-500" type="submit"
                @click="register">Register</button>
            <button class="p-2 rounded-sm bg-white text-black" @click="signUpWithGoogle">Log in with
                Google</button>
        </form>
    </div>
    <div class="flex justify-center my-1">
        <div class="m-3 p-2 bg-slate-900 w-fit flex flex-col">
            <p class="text-white" :class="[validateEmail(emailInput)===true ? 'text-green-300' : 'text-white' ]">
                &check; Email is valid
            </p>
            <p :class="[validatePassLength(passInput)===true ? 'text-green-300' : 'text-white' ]">&check; Password is
                atleast 8
                characters</p>
            <p :class="[validatePassMatch(passInput, confirmPassInput)===true ? 'text-green-300' : 'text-white' ]">
                &check;
                Passwords
                match</p>
        </div>
    </div>
    <div>
        <p>{{ returnMsg }}</p>
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