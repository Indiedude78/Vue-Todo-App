<script setup>
import { auth } from '../firebase'
import { signOut } from "firebase/auth";
import { useRouter } from 'vue-router'
const router = useRouter()

signOut(auth).then(() => {
    // Sign-out successful.
    console.log("signed out")
    router.push('/login')
}).catch((error) => {
    // An error happened.
});
</script>