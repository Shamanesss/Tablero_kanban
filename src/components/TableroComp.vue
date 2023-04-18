<template>
  <nav class="navbar">
    <button @click.prevent="handleNewTablero">Crear Tablero</button>
    <input type="text" id="newTablero" />
  </nav>

  <div class="tablero-container">
    <div class="tableros">
      <div
        class="tablero"
        @drop="cogerSoltar($event, tablero)"
        @dragover.prevent
        @dragenter.prevent
        v-for="tablero in tableros"
        :key="tablero.id"
      >
        <div>{{ tablero.name }}</div>
        <AgregarInputComp
          @nuevo-item="(text) => handleNewItem(text, tablero)"
        />
        <!-- Aquí aplicamos el emit creado en AgregarInputComp -->
        <div class="items">
          <div
            class="item"
            draggable="true"
            @dragstart="cogerArrastrar($event, tablero, item)"
            v-for="item in tablero.items"
            :key="item.id"
          >
            <!-- La propiedad draggable existe en todos los elementos HTML y sirve para poder moverlos de un sitio a otro -->
            <div>{{ item.title }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive } from "vue";
import AgregarInputComp from "./AgregarInputComp.vue";
const tableros = reactive([
  {
    id: crypto.randomUUID(),
    name: "Tablero 1",
    items: [
      {
        id: crypto.randomUUID(),
        title: "Caracteristicas de Archivos",
      },
      {
        id: crypto.randomUUID(),
        title: "Resolver Bugs",
      },
    ],
  },
  {
    id: crypto.randomUUID(),
    name: "Tablero 2",
    items: [
      {
        id: crypto.randomUUID(),
        title: "Revisar Codigo",
      },
      {
        id: crypto.randomUUID(),
        title: "Realizar Figma",
      },
    ],
  },
]);
function handleNewItem(text, tablero) {
  tablero.items.push({
    id: crypto.randomUUID(),
    title: text.value,
  });
  console.log(text.value, tablero.id, tablero.name);
}

//Crear un nuevo tablero
function handleNewTablero() {
  // const name = prompt("Nombre del Tablero");
  const input = document.getElementById("newTablero");
  const name = input.value;
  if (name !== "") {
    tableros.push({
      id: crypto.randomUUID(),
      name: name,
      items: [],
    });
  }
} // funcion para mover los items de un sitio a otro

function cogerArrastrar(e, tablero, item) {
  e.dataTransfer.setData(
    "text/plain",
    JSON.stringify({ tableroId: tablero.id, itemId: item.id })
  );
}

//funcion para soltar los items en el tablero

function cogerSoltar(e, dest) {
  const { tableroId, itemId } = JSON.parse(
    e.dataTransfer.getData("text/plain")
  );
  const originBoard = tableros.find(
    (item) => item.id === tableroId
  ); /* Tengo el tablero de origen */
  const originItem = originBoard.items.find(
    (item) => item.id === itemId
  ); /* Tengo la tarjeta del origen*/

  dest.items.push({ ...originItem });
  originBoard.items = originBoard.items.filter((item) => item !== originItem);
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
button {
  background: blanchedalmond;
}
.navbar {
  background-color: black;
  color: antiquewhite;
  padding: 2rem;
}
.tablero-container {
}

.tableros {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 2rem;
  margin: 2rem;
}
.tablero {
  background-color: blanchedalmond;
  padding: 0.8rem;
}
.items {
  gap: 1rem;
  display: flex;
  flex-direction: column;
}
.item {
  background-color: azure;
  padding: 0.8rem;
  box-sizing: border-box;
}
</style>
