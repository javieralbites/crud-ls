<template>
  <div class="maskModal">
    <div class="modal">
      <h2>Editar Nota</h2>
      <h4 for="titulo">Título</h4>
      <input id="titulo" type="text" v-model="tituloEditado" />

      <h4 for="descripcion">Descripción</h4>
      <textarea
        name="nuevaDescripcion"
        id="nuevaDescripcion"
        v-model="descripcionEditado"
        cols=""
        rows="5"
      >
      </textarea>

      <div class="botones">
        <button @click="cerrarModalEditar">Cancelar</button>
        <button @click="guardarNotaEditada">Guardar</button>
      </div>
    </div>
  </div>
</template>

<script>
import Localbase from "localbase";
let db = new Localbase("db");
export default {
  data() {
    return {
      tituloFinal: "",
      descripcionFinal: "",
    };
  },
  props: {
    tituloEditar: String,
    descripcionEditar: String,
    idEditar: Number,
  },
  methods: {
    cerrarModalEditar: function () {
      this.$emit("cerrarModalEditar");
    },
    guardarNotaEditada: function () {
      //   let task = this.tasks.find(task => task.id === id)
      db.collection("notas")
        .doc({ id: this.idEditar })
        .update({
          titulo: this.tituloFinal || this.tituloEditado,
          descripcion: this.descripcionFinal || this.descripcionEditado,
        });
      this.cerrarModalEditar();
      location.reload();
      //   task.done = !task.done
    },
  },
  computed: {
    tituloEditado: {
      get: function () {
        return this.tituloEditar;
      },
      set: function (newValue) {
        this.tituloFinal = newValue;
      },
    },
    descripcionEditado: {
      get: function () {
        return this.descripcionEditar;
      },
      set: function (newValor) {
        this.descripcionFinal = newValor;
      },
    },
  },
};
</script>

<style scoped>
.maskModal {
  background-color: rgba(0, 0, 0, 0.7);
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 9998;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal {
  width: 30%;
  background: #25273c;
  padding: 1em;
  color: #fff;
  border: #fff solid 2px;
  border-radius: 15px;
}
@media (max-width: 1200px) {
  .modal {
    width: 70%;
  }
}
@media (max-width: 680px) {
  .modal {
    width: 90%;
  }
}
h2 {
  margin-bottom: 1em;
}
input,
textarea {
  width: 100%;
  font-size: 1em;
  padding: 0.3em;
}
.botones {
  display: flex;
  justify-content: space-between;
  gap: 1em;
  margin-top: 1em;
}
.botones button {
  width: 100%;
  padding: 0.3em;
  font-size: 1em;
}
button:disabled {
  cursor: not-allowed;
}
</style>