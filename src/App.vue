<template>
  <div class="body">
    <div class="imagen">
      <img
        class="img"
        src="https://www.japonalternativo.com/wp-content/uploads/2020/03/tiendas-de-anime-y-manga-Tokio.jpg"
        alt=""
      />
    </div>
    <div class="barra">
      <div class="nombre">
        <h2>Ciudad Mangas</h2>
      </div>
      <div class="boton">
        <button @click="toggleMostrar" class="boton1">
          <img class="carri" src="/src/img/carrito.png" alt="" />
        </button>
      </div>
      <div class="table-contenedor">
      <table v-if="mostrar">
        <thead>
          <tr>
            <th class="titulo">imagen</th>
            <th class="titulo">nombre</th>
            <th class="titulo">precio</th>
            <th class="titulo">Cantidad</th>
            <th class="titulo">Subtotal</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in carrito" :key="index">
            <td><img class="primera" :src="item.img" alt="" /></td>
            <td>{{ item.producto }}</td>
            <td>{{ item.precio }}</td>
            <td>
            <button class="boton3" @click="quitarUno(index)">-</button>
              {{ item.cantidad }}
            <button class="boton3" @click="agregarUno(index)">+</button>
            </td>
            <td>{{formatoMonedaLocal( convertir(item.precio) * item.cantidad) }}</td>
            <td><button class="boton2" @click="eliminar(index)">❌</button></td>
          </tr>
          <td><h4 @click="vaciar">Vaciar carrito</h4></td>
          <td></td>
          <td></td>
          <td class="total">Total:</td>
          <td class="total"> {{ formatoMonedaLocal(calcularTotal()) }}</td>
        </tbody>
      </table>
    </div>
  </div>
    <div class="ti"><h1 class="titulo2">Mangas disponibles</h1></div>
    <div class="cata">
      <div v-for="(item, index) in data" :key="index" class="tarjeta">
        <img class="segundo" :src="item.img" alt="" />
        <p>{{ item.producto }}</p>
        <p>{{ item.dueño }}</p>
        <p>$ {{ item.precio }}</p>
        <div class="estrellas">
          <span v-for="n in calcularEstrellas(item.puntuacion)" :key="n">⭐</span>
        </div>
        <button class="botoncar" @click="agregar(item)">
          Agregar al carrito
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import given from "/src/img/given.png";
import one from "/src/img/one_piece.webp";
import seven from "/src/img/seven.webp";
import banana from "/src/img/banana.jpg";
import hero from "/src/img/hero.webp";
import koro from "/src/img/koro.webp";
import kaisen from "/src/img/kaisen.webp";
import horimiya from "/src/img/horimiya.png";
import yuri from "/src/img/yuri.jpg";
import ghoul from "/src/img/ghoul.webp";
import koe from "/src/img/koe.jpg";
import bungou from "/src/img/bungou.webp";



const mostrar = ref(false);
const carrito = ref([]);

const data = ref([
  {
    img:given,
    producto: "Given",
    dueño: "Juan",
    precio: "50.000",
    puntuacion: 5,
  },
  {
    img: one,
    producto: "One Piece",
    dueño: "Juan",
    precio: "35.000",
    puntuacion: 1,
  },
  {
    img: seven,
    producto: "Nanatsu No Taizai",
    dueño: "Juan",
    precio: "45.000",
    puntuacion: 4,
  },
  {
    img: banana,
    producto: "Banana Fish",
    dueño: "Juan",
    precio: "55.000",
    puntuacion: 5,
  },
  {
    img: hero,
    producto: "My Hero Academia ",
    dueño: "Juan",
    precio: "45.000",
    puntuacion: 4,
  },
  {
    img: koro,
    producto: "Assassination Classroom",
    dueño: "Juan",
    precio: "52.000",
    puntuacion: 5,
  },
  {
    img: kaisen,
    producto: "Jujutsu Kaisen",
    dueño: "Juan",
    precio: "38.000",
    puntuacion: 3,
  },
  {
    img: horimiya,
    producto: "Horimiya",
    dueño: "Juan",
    precio: "47.000",
    puntuacion: 5,
  },
  {
    img: yuri,
    producto: "Yuri on Ice",
    dueño: "Juan",
    precio: "60.000",
    puntuacion: 5,
  },
  {
    img: ghoul,
    producto: "Tokyo Ghoul",
    dueño: "Juan",
    precio: "31.000",
    puntuacion: 2,
  },
  {
    img: koe,
    producto: "Koe No Katachi",
    dueño: "Juan",
    precio: "30.000",
    puntuacion: 3,
  },
  {
    img: bungou,
    producto: "Bungou Stray Dogs",
    dueño: "Juan",
    precio: "42.000",
    puntuacion: 4,
  },
]);

function calcularEstrellas(puntuacion) {
  return Math.round(puntuacion);
}

function agregar(item) {
  const carritoItem = carrito.value.find(
    (cartItem) => cartItem.img === item.img
  );
  if (carritoItem) {
    carritoItem.cantidad++;
  } else {
    carrito.value.push({ ...item, cantidad: 1 });
  }
}

function toggleMostrar() {
  mostrar.value = !mostrar.value;
}

function eliminar(index) {
  carrito.value.splice(index, 1);
}

function vaciar() {
  carrito.value = [];
}

function calcularTotal() {
  return carrito.value.reduce(
    (total, item) => total + convertir(item.precio )* item.cantidad,
    0
  );
}

const convertir=(valor)=>{
  return parseInt(valor.replace(/[^0-9-]/g, ''));
}

const formatoMonedaLocal = (numero) => {
  return numero.toLocaleString('es-CO', {
    style: 'currency',
    currency: 'COP',
    minimumFractionDigits: 0,
    maximumFractionDigits: 0,
  });
};

function agregarUno(index) {
  carrito.value[index].cantidad++;
}

function quitarUno(index) {
  if (carrito.value[index].cantidad > 1) {
    carrito.value[index].cantidad--;
  }
}
</script>

<style scoped>
.body {
  background-color: rgb(235, 233, 233);
}

.primera {
  height: 100px;
  height: 100px;
}

.segundo {
  width: 100%;
  height: 55%;
}

.cata {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(373px, 1fr));
  gap: 10px;
}

.tarjeta {
  width: 350px;
  height: 590px;
  border: 2px solid black;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 10px;
  margin-left: 8%;
  background-color: white;
  margin-bottom: 8px;
  font-size: 20px;
}

.botoncar {
  margin-top: 15px;
  padding: 10px 20px;
  font-size: 16px;
  background-color: #3498db;
  color: white;
  border: none;
  cursor: pointer;
}

.barra {
  margin: 0;
  height: 55px;
  width: 100%;
  background-color: white;
  display: flex;
  position: fixed;
  top: 0;
  justify-content: space-evenly;
  align-items: center;
}

.table-contenedor {
  margin-top: 525px;
  height: 475px;
  max-height: 475px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

table {
  width: 500px;
  flex: 1;
  justify-content: center;
  text-align: center;
  font-size: 18px;
  border-collapse: collapse;
  background-color: white;
}

td,
tr {
  border-bottom: 1px solid rgb(201, 199, 199);
}

.titulo {
  padding: 10px;
  font-size: 20px;
}
.estrellas {
  display: flex;
  flex-direction: row;
}

.nombre {
  font-size: 32px;
  position: fixed;
  left: 35px;
}
.imagen {
  text-align: center;

}

.img {
  width: 100%;
  max-height: 620px;
}
.titulo2 {
  font-size: 55px;
  text-align: center;
  color: #3498db;
}
.carri {
  width: 50px;
  height: 50px;
}

.boton1 {
  background-color: transparent;
  border: 1px solid transparent;
  position: fixed;
  top: 6px;
  right: 30%;
}

.boton3{
  width: 29px;
  height: 30px;
  font-size: 25px;
  font-weight: bold;
  background-color: transparent;
}

.boton2{
  width: 50px;
  height: 30px;
  margin-right: 20px;
  background-color: transparent;
}

h4 {
  margin: 0;
  cursor: pointer;
}

.total {
  font-weight: bold; 
}

/* Tu CSS general aquí */

@media screen and (max-width: 900px){
  .cata {
    grid-template-columns: repeat(auto-fit, minmax(261px, 1fr)); 
  }

  .tarjeta {
    width: 100%; 
    margin-left: 0;  
  }

  .imagen {
    margin-top: 20px; 
  }

  .boton1{
    right: 15%;
  }

  .table-contenedor {
  margin-top: 525px;
  height: 475px;
  max-height: 475px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
}

table {
  width: 50px;
  flex: 1;
  justify-content: center;
  text-align: center;
  font-size: 18px;
  border-collapse: collapse;
  background-color: white;
}
}

</style>