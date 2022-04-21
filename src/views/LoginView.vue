<template>
    <div class="container w-50"  style="background-color: #F6F6F7; padding-bottom: 20px; border: 2px solid green">
        <div class="row justify-content-center">
                <h1 class=" mt-3">Login</h1>
            <form class="col-12 col-lg-8" action="">
                <div class="mb-3">
                    <label for="Email" class="form-label">Email :</label>
                    <input type="email" class="form-control w-100" id="Email" v-model="email" required>
                </div>
                <div class="mb-3">
                    <label for="Password" class="form-label">Mot de passe :</label>
                    <input type="password" class="form-control w-100" id="Password" v-model="password" required>
                </div>
                <p style="color: red;">{{error}}</p>
                <button type="submit" class="btn btn-success  mt-2" v-on:click.prevent.stop="login()">Se connecter</button>
            </form>
        </div>
    </div>
</template>
<script>
import { ref } from '@vue/reactivity';
import { useRouter } from 'vue-router';
import { onMounted } from '@vue/runtime-core';

export default{
    props: ["auth", "setAuth"],
    setup(props, context) {
        const router = useRouter();
        const email = ref("");
        const password = ref("");
        const error = ref("")
        onMounted(() => {
            if (props.auth.authentified) {
                router.push('/');
            }
        })
        async function login(){
            const users = await fetch('http://localhost:3001/users').then(resp => resp.json());
            let user = users.find((us) => us.email === email.value);
            if (user && user.password === password.value) {
                error.value = "";
                props.setAuth(user, true);
                router.push("/");
            }
            else{
                error.value = "Vos identifiants ne correspondent pas.";
            }
        }
        return {email, password, error, login, router}
    },
}
</script>
