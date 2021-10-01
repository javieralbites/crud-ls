<template>
  <div id="app">
    <h1 class="title">crud localstorage</h1>
    <div class="control">
      <span @click="abrirModal" class="agregarTarea">
        + Agregar tarea</span
      >
    </div>

    <div class="container">
      <Card
        v-for="libro in notas"
        :key="libro.id"
        :titulo="libro.titulo"
        :descripcion="libro.descripcion"
        :fecha="libro.fecha"
        :id="libro.id"
        @eliminarNota="eliminarNota"
        @editarNota="editarNota"
      />
    </div>
    <template v-if="modalAgregar">
      <ModalAgregar
        @agregarNota="agregarNota"
        @cerrarModalAgregar="cerrarModalAgregar"
      />
    </template>
    <template v-if="modalEditar">
      <ModalEditar
        @cerrarModalEditar="cerrarModalEditar"
        :tituloEditar="tituloEditar"
        :descripcionEditar="descripcionEditar"
        :idEditar="idEditar"
      />
    </template>
  </div>
</template>

<script>
import Localbase from "localbase";
let db = new Localbase("db");
import Card from "./components/Card";
import ModalAgregar from "./components/ModalAgregar";
import ModalEditar from "./components/ModalEditar";
export default {
  name: "App",
  components: {
    Card,
    ModalAgregar,
    ModalEditar,
  },
  data() {
    return {
      nuevoTitulo: "",
      nuevaDescripcion: "",
      modalAgregar: false,
      modalEditar: false,
      tituloEditar: "",
      descripcionEditar: "",
      idEditar: "",
      notas: [],
    };
  },
  methods: {
    obtenerTareas: function () {
      db.collection("notas")
        .get()
        .then((notas) => {
          this.notas = notas;
        });
    },
    abrirModal: function () {
      this.modalAgregar = true;
    },
    agregarNota: function (nuevaNota) {
      this.notas.push(nuevaNota);
      db.collection("notas").add(nuevaNota);
      this.modalAgregar = false;
    },
    cerrarModalAgregar: function () {
      return (this.modalAgregar = false);
    },
    eliminarNota: function (id) {
      let index = this.notas.findIndex((nota) => nota.id === id);
      db.collection("notas").doc({ id: id }).delete();
      this.notas.splice(index, 1);
    },
    editarNota: function (titulo, descripcion, id) {
      this.modalEditar = true;
      (this.tituloEditar = titulo), (this.descripcionEditar = descripcion);
      this.idEditar = id;
    },
    cerrarModalEditar: function () {
      return (this.modalEditar = false);
    },
  },
  created() {
    this.obtenerTareas();
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700;900&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
}
body {
  background: #25273c;
}
.title {
  text-align: center;
  font-weight: 700;
  font-size: 3em;
  text-transform: uppercase;
  padding: 1em;
  color: #fff;
}
.control {
  width: 80%;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1em;
}
.agregarTarea {
  box-shadow: inset 0px 0px 15px 3px #23395e;
  background: linear-gradient(to bottom, #2e466e 5%, #415989 100%);
  background-color: #2e466e;
  border-radius: 17px;
  border: 1px solid #1f2f47;
  display: inline-block;
  cursor: pointer;
  color: #ffffff;
  font-family: Arial;
  font-size: 1.3em;
  padding: 0.8em 1em;
  text-decoration: none;
  text-shadow: 0px 1px 0px #263666;
  border: #fff solid 2px;
}
.agregarTarea:hover {
  background: linear-gradient(to bottom, #415989 5%, #2e466e 100%);
  background-color: #415989;
}
.agregarTarea:active {
  position: relative;
  top: 1px;
}
.container {
  width: 80%;
  margin: 0 auto;
  padding-bottom: 2em;
  columns: 3;
  gap: 2em;
  /* row-gap: 1em; */
}
@media (max-width: 1200px) {
  .container {
    columns: 2;
    width: 90%;
  }
}
@media (max-width: 580px) {
  .container {
    columns: 1;
  }
  .title {
    font-size: 1.7em;
  }
}
</style>