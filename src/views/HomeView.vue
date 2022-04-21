<template>
    <div class="container">
        <h1>MyNetWork</h1>
        <div v-if="auth.authentified">
            <button class="btn btn-danger" v-on:click="setAuth(null, false)">Déconnexion</button>
        </div>

        <div v-else>
            <RouterLink class="btn btn-dark mb-3 text-white" to="/login">Login</RouterLink><br>
            <RouterLink class="btn btn-dark text-white" to="/register">Register</RouterLink>
        </div>
                
            
        <FormArticles @add="addArticle" v-if="auth.authentified" :auth="auth"/>
        <Articles :auth="auth" :articles="articles"></Articles>
    </div>
</template>
<script>
import { ref, onMounted } from 'vue';
import FormArticles from '../components/FormArticles.vue';
import Articles from '../components/Articles.vue';

export default {
    props: ["auth", "setAuth"],
    setup(props, context){
        const articles = ref([]);
        onMounted(async () => {
            articles.value = await fetch('http://localhost:3001/articles').then(resp => resp.json());
        });
        async function addArticle(article){
            articles.value.push(article);
        }
        return {articles, addArticle}
    },
    components: {FormArticles, Articles}
}
</script>