<template>
    <div class="row justify-content-center" st>
        <form class="col-12 col-lg-8" action="">
            <h2>Quoi de neuf ?</h2>
            <div class="mb-3">
                <textarea class="form-control" placeholder="Mon nouvel article" v-model="body"></textarea>
            </div>
            <button class="btn btn-success" type="submit" v-on:click.prevent.stop="addArticle()">Envoyer mon article</button>
        </form>
    </div>
</template>
<script>
import { ref } from '@vue/reactivity'
export default {
    props: ["auth"],
    emits: ['add'],
    setup(props, context) {
        const body = ref("");
        async function addArticle(){
            if (body.value) {
                const article = {
                    contenu: body.value, 
                    urlImgArticle: "https://source.unsplash.com/random/1000x300",
                    like: 0,
                    pseudo: props.auth.user.pseudo,
                    date: Date.now(),
                    commentaires: []
                };
                const response = await fetch(`http://localhost:3001/articles`, {headers: {'Accept': "application/json", 'Content-type': 'application/json'},method: "POST", body: JSON.stringify(article)}).then(resp => resp.json());
                context.emit('add', response);
                body.value = "";
            }
        }
        return {body, addArticle}
    },
}
</script>


<style scoped>
form{
    background: #f1f5f8;
    padding: 16px 24px;
    border: 4px solid black;
}
</style>