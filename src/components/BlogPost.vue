<template>
  <div class="card">
    <div class="card-body">
      <h5 class="card-title" :class="`text-${colorText}`">
        {{id}} - {{title}}

      </h5>



      <p>{{body}}</p>
      <!-- <button @click="$emit('cambiarFavoritoNombre',title)" class="btn btn-outline-primary"> Favorito</button> -->
      <button @click="emit('cambiarFavoritoNombre',title)" class="btn btn-outline-primary"> Favorito</button>

    </div>
  </div>
</template>

<script setup>
defineProps(["title", "id", "body", "colorText"])//esta es la opcion mas estandarizada
// defineProps({
//   title: String,
//   id: Number,
//   body: {
//     type: String,
//     default: "Sin descripción"
//   },
//   colorText: String
// }) //esta alternativa es opcional para pasar props en forma de objeto

//el signo $ activa metodos globales, si no queremos activar estos globales podemos usar el defineEmits(),esto recibe un array y dentro el nombre del parametro del cmponente App al que queremos referirnos.Asi ya no tenemos que poner en el @click del boton el signo $emit, solo emit(osea la constante) la cual devuelve un objeto.
const emit = defineEmits(["cambiarFavoritoNombre"])


</script>

<style>

</style>