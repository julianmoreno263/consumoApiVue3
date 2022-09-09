<template>

  <LoadingSpinner v-if="loading" />
  <div class="container" v-else>
    <h1>APP</h1>
    <h2>Mis post favoritos: {{favorito}}</h2>
    <!-- <ButtonCounter />
    <ButtonCounter /> -->



    <PaginatePost class="mb-2" @next="next" @prev="prev" :inicio="inicio" :fin="fin" />

    <BlogPost v-for="post in posts.slice(inicio,fin)" :key="post.id" :title="post.title" :id="post.id" :body="post.body"
      :colorText="post.colorText" @cambiarFavoritoNombre="cambiarFavorito" class="mb-2" />
    <!-- <BlogPost title="Post 2" :id="2" body="descripción 2" colorText="secondary" />
    <BlogPost title="Post 3" :id="3" body="descripción 3" colorText="success" />
    <BlogPost title="Post 4" :id="4" body="descripción 4" colorText="primary" /> -->


  </div>
</template>

<script setup>
import { onMounted, ref } from "vue"
//el setup da azucar sitactico en js, osea sirve para abreviar el codigo y no tener que escribir el export, es para ayudar a escribir menos y mas limpio. Esto es para el composition API.

//Para utilizar un componente en el componente principal App.vue debemos importarlo(sin las {}), y ya importado en el script lo usamos en el template como una etiqueta(igual que en react, entre < />), en el componente no hay que hacer export porque la propiedad setup ya hace el export por default(sintax sugar)

import ButtonCounter from './components/ButtonCounter.vue';
import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';

//podemos recorrer los props si los metemos en un array reactivo con ref(), luego en el template con un v-for los recorremos y los hacemos dinamicos colocando los dos puntos antes de cada prop, asi se trabaja normalmente y es mejor el codigo, es mas limpio y por ejemplo si consumimos datos de una api se recorren asi.
// const posts = ref([
//   { title: "Post 1", id: 1, body: "descripción 1", colorText: "primary" },
//   { title: "Post 2", id: 2, body: "descripción 2", colorText: "secondary" },
//   { title: "Post 3", id: 3, body: "descripción 3", colorText: "success" },
//   { title: "Post 4", id: 4, body: "descripción 4", colorText: "primary" },

// ])


//si queremos comunicar un componente secundario (como uno de los post) con el componente principal App, por ejemplo que en el componente de post haya un boton que al darle click active el h2 que tiene el Appvue, esto lo realizamos con emit. En si, debemos hacer que el boton del componente post ejecute una funcion que esta en el componente App, esto se llama "emisiones de eventos o emit", osea comunicacion entre componentes.Entonces, en el template de App donde llamamos a el componente BlogPpost le pasamos un parametro con la sintaxis de @ y le damos un nombre  a este parametro, y el valor sera la funcion que creamos en App.vue que queremos ejecutar.Despues ya en el componente BlogPost podemos llamar a esta funcion desde el boton que creamos con un @click y utilizamos el metodo emit() pero con esta sintaxis: $emit("nombre del parametro","parametro de la funcion"), al emit se le pasa el nombre del parametro y como segundo parametro el argumento de esa funcion. Ahora si el titulo que esta en el componente App sera dinamico,osea lo especificamos asi: {{favorito}}.No olvidar que se debe usar el metodo ref() para que sea dinamico. Asi se hace la comunicacion entre componentes, con pinia es mas facil.

//ahora, este metodo de App.vue lo podiamos tambien pasar al componente Blog como parametro, sin usar el emit. simplemnete lo nombramos :cambiarFavoritoNombre, ya con estos dos puntos se sabe que es un parametro, y en los defineProps del Blog lo paso, ahora esto no se sugiere hacerlo,se sugiere hacer el emit,porque si pasamos las funciones y estas llegan aser muy grandes el codigo se nos complica, por eso vue se invento los emits.
// const favorito = ref("")

// const cambiarFavorito = (title) => {
//   favorito.value = title
// }



//------------------------------  practica -------------------------------------------------------

const posts = ref([])

//constantes para la paginacion
const postXPagina = 10
const inicio = ref(0)
const fin = ref(postXPagina)
const loading = ref(true)


const favorito = ref("")

const cambiarFavorito = (title) => {
  favorito.value = title
}

//metodo para la paginacion
const next = () => {
  inicio.value = inicio.value + postXPagina
  fin.value = fin.value + postXPagina
}

const prev = () => {
  inicio.value = inicio.value - postXPagina
  fin.value = fin.value - postXPagina
}

//como usamos el setup, este tiene el metodo create el cual lo que hace es ver toda la logica(codigo js) que tenemos y lo crea, y despues se monta el html(osea el sitio web), pero hay otros ciclos de vida que podemos controlar,uno es el "onMounted"  el cual una vez el template este montado podemos activar un metodo especifico, es como un reemplazo al create. El onMounted recibe una funcion de callback la cual se ejecuta una vez esta montado el template.Entonces con esto podemos tener otra opcion al fetch para cargar la API.El onMounted es un poco mas ordenado que hacer solo el fetch directamente, ademas el espera que se monte primero el template y despues si hace la peticion. Pero el profe dice que lo mejor en vez del onMounted es el fetch directamente porque cuando se esta cargando el template se va haciendo la solicitud de una vez. Pero dice que la mejor alternativa a estas dos es el fetch dentro de un try-catch con async-await,osea creamos una funcion y esa funcion sera async-await dentro de un try-catch(son alternativas de hacer las cosas pero la mejor es con async-await para manejar el llamado a las APIS con el fetch.) de esta forma cuando se crea el componente(osea se monta el template), se crea la funcion async y se le llama.

// onMounted(async () => {

//   try {
//     const res = await fetch("https://jsonplaceholder.typicode.com/posts")
//     posts.value = await res.json()
//   } catch (error) {
//     console.log(error)
//   } finally {
//     setTimeout(() => {
//       loading.value = false

//     }, 2000)
//   }
// })

//consumir API de jsonPlaceHolder con fetch
// fetch("https://jsonplaceholder.typicode.com/posts")
//   .then(res => res.json())
//   .then((data) => {
//     posts.value = data
//   })
//   .catch((error) => console.log(error))
//   .finally(() => {
//     setTimeout(() => {
//       loading.value = false

//     }, 2000)
//   })



//mejor alternativa con async-await para consumir APIS
const fetchData = async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts")
    posts.value = await res.json()
  } catch (error) {
    console.log(error)
  } finally {
    setTimeout(() => {
      loading.value = false

    }, 2000)
  }
}

fetchData()


</script>




<style>

</style>