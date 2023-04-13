<script setup>
import ButtonCounter from './components/ButtonCounter.vue'
import BlogPost from './components/BlogPost.vue'
import PaginatePost from "./components/PaginatePost.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";
import { ref, computed, onMounted } from 'vue';

const posts=ref([]);
const favorito=ref('')
const postXpage=(10)
const inicio=ref(0);
const fin=ref(postXpage);
const loading=ref(true)
const cambiarFavorito= (title)=>{
  favorito.value=title;
}

const next=() => {
  inicio.value = inicio.value + postXpage;
    fin.value = fin.value + postXpage;
}
const previus=()=>{
  inicio.value=inicio.value-postXpage;
  fin.value=fin.value-postXpage;
}

onMounted(async()=>{
  try {
    const res=await fetch("https://jsonplaceholder.typicode.com/posts")
    posts.value=await res.json()
  } catch (error) {
    console.log(error);
  } finally{
    setTimeout(()=>{
      loading.value=false
    }, 2000)
  }
})

// fetch("https://jsonplaceholder.typicode.com/posts")
//   .then((res)=>res.json())
//   .then((data)=>posts.value=data)
//   .catch((e)=>console.log(e))
//   .finally(()=> {
//     setTimeout(()=>{
//       loading.value=false
//     }, 2000)
//   })

const maxLength=computed(()=> posts.value.length)
</script>

<template>
  <LoadingSpinner v-if="loading"></LoadingSpinner>
  <div v-else class="container">
    <h1>APP</h1>
    <h2>Mis Post Favorito: {{ favorito }}</h2>
    <!-- <BlogPost title="Post 1" id="1" body="Saludos" colorText="primary"/>
    <BlogPost title="Post 2" id="2" body="Saludos" colorText="warning"/>
    <BlogPost title="Post 3" id="3" body="Saludos" colorText="success"/> -->
    <!-- <button @click="next">Siguiente</button>
    <button @click="previus">Antes</button> -->
    <PaginatePost @next="next" @previus="previus" :inicio="inicio" :fin="fin" :maxLength="maxLength" class="mb-2" ></PaginatePost>
    <BlogPost  
      v-for="post in posts.slice(inicio,fin)"
      :key="post.id"
      :title="post.title" 
      :id="post.id" 
      :body="post.body"
      :cambiarFavorito="cambiarFavorito"
      class="mb-2"
    ></BlogPost>
  </div>
</template>