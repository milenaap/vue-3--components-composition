<script setup>
import { ref, computed, onMounted } from 'vue';
import BlogPost from './components/BlogPost.vue'
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';


/*  const posts = ref([
    {title: 'Post 1', id: 1, body: 'descripcion 1'},
    {title: 'Post 2', id: 2, body: 'descripcion 2'},
    {title: 'Post 3', id: 3, body: 'descripcion 3'},
    {title: 'Post 4', id: 4, },
]) */

const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(true);

const favorito = ref('');
const cambiarFavorito = (title) => {
    favorito.value = title;
}

const next = () => {
    inicio.value = inicio.value + postXpage;
    fin.value = fin.value + postXpage;
    }

const prev = () => {
    inicio.value = inicio.value - postXpage;
    fin.value = fin.value - postXpage;
}


/* onMounted (() => {
    fetchData();    
}); */

/* fetch('https://jsonplaceholder.typicode.com/posts')
    .then(res => res.json())
    .then(data => {
        posts.value= data
    })
    .catch (e => console.log(e))
    .finally(() => {
        setTimeout (() => {
            loading.value = false
        }, 2000)
        
    }) */


    const fetchData = async() => {
        try {
            const res = await fetch('https://jsonplaceholder.typicode.com/posts')
            posts.value = await res.json()
        } catch (error) {
            console.log(error)
            
        }finally {
                setTimeout (() => {
                loading.value = false
            }, 2000)
        }
    }


    fetchData();


const maxLength = computed(() => posts.value.length)
    
    
</script>

<template>
    <LoadingSpinner v-if="loading" />
    <div class="container" v-else> 

        <h1>APP</h1>
        <h2>Mi Post Favorito: {{ favorito }}</h2>
             
        <PaginatePost 
             
            @next="next"
            @prev="prev"
            :inicio="inicio"
            :fin="fin"
            :maxLength="maxLength"
            class="mb-2"
            />

            
        
        <BlogPost 
            v-for="post in posts.slice(inicio, fin)"
            :key="post.id"
            :title="post.title"
            :id="post.id"
            :body="post.body"
            @cambiarFavorito="cambiarFavorito"
            class="mb-2"  
        ></BlogPost>

    

    </div>

</template>
