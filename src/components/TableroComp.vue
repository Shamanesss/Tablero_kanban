<template>
  <nav><a href='#' @click.prevent="handleNewTablero">Crear tablero</a></nav>

 <div class="tablero_container">
  <div class="tableros">
    <div class="tablero" @drop="cogerSoltar($evento, tablero)" @dragover.prevent @dragenter.prevent v-for="tablero in tableros" :key="tablero.id">
      <div>{{ tablero.name }}</div>
      <AgregarInputComp @nuevo-item="(text) => handleNewItem(text, tablero)" />
        <div class="items">
          <div class="item" draggable="true" @dragstart="cogerArrastrar($evento, tablero, item)" v-for="item in tablero.items" :key="item.id">
            {{ item.title }}
          </div>
        </div>
    </div>
  </div>
</div>
</template>

<script setup>
import { reactive } from 'vue'
import AgregarInputComp from './AgregarInputComp.vue';
const tableros = reactive([
  {
    id: crypto.randomUUID(),
    name: 'Tablero 1',
    items:[
      {
        id:crypto.randomUUID(),
        title:'Caracteristicas de Archivos',
      },
      {
        id:crypto.randomUUID(),
        title:'Resolver Bugs'
      }
    ],
  },
  {
    id: crypto.randomUUID(),
    name: 'Tablero 2',
    items:[
      {
        id: crypto.randomUUID(),
        title:'Revisar Codigo',
      },
      {
        id:crypto.randomUUID(),
        title:'Realizar Figma'
      }
    ],
  },
]);
function handleNewItem(text, tablero){
tablero.items.push({
  id:crypto.randomUUID(),
  title:text.value,
});
  console.log(text.value, tablero.id, tablero.name)
}
function handleNewTablero(){
  const name = prompt('Nombre del Tablero');
  if(name != ''){
    tableros.push({
      id: crypto.randomUUID(),
      name:name,
      items:[],
    });
  }
}
// function cogerArrastrar(evento, tablero, item ){
// evento.dataTransfer.setData(
//   "text/plain",
//   JSON.stringify({tableroId: tablero.id, itemId: item.id})
// )
//}
function cogerArrastrar(event, tablero, item) {
  if (event.currentTarget.classList.contains('item')) {
    event.dataTransfer.setData(
      "text/plain",
      JSON.stringify({tableroId: tablero.id, itemId: item.id})
    )
  }
}


function cogerSoltar(evento, destino){
const {tableroId, itemId}=JSON.parse(evento.dataTransfer.getData('text/plain'));
console.log(tableroId, itemId)
const origenTablero= tableros.find((tablero) => tablero.id === tableroId);
const origenItem = origenTablero.items.find((item) => item.id === itemId);
destino.items.push({...origenItem})
origenTablero.items =origenTablero.items.filter((item) =>item !=origenItem)
console.log(origenItem.title, origenTablero.name)
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.tableros{
  display: flex;
  gap: 2rem;
}
.tablero{
  background-color: blanchedalmond;
  padding: 0.8rem;
  
}
.items{
  gap:1rem;
  display: flex;
  flex-direction: column;
}
.item{
  background-color: azure;
  padding: 0.8rem;
  box-sizing: border-box;
  
}
</style>