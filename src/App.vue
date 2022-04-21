<template>
<div class="container mt-5">
    <RouterView :auth="auth" :setAuth="setAuth"/></div>
</template>

<script>
import { ref } from '@vue/reactivity';
import { RouterView, useRouter } from 'vue-router';

export default {
    setup(){
        const auth = ref({authentified: false, user: null});
        const router = useRouter();
        function setAuth(user, bool){
            auth.value.authentified = bool;
            auth.value.user = user;
            !bool && (router.push('/'));
        };
        return {auth, setAuth}
    }
}


</script>
