<template>
  <div class="maskModal">
    <div class="modal">
      <h2>Agregar nueva nota</h2>
      <h4 for="titulo">Título</h4>
      <input id="titulo" type="text" v-model="nuevoTitulo" />
      <h4 for="descripcion">Descripción</h4>
      <textarea
        name="nuevaDescripcion"
        id="nuevaDescripcion"
        v-model="nuevaDescripcion"
        cols=""
        rows="5"
      ></textarea>
      <div class="botones">
        <button @click="cerrarModal">Cancelar</button>
        <button @click="agregarNota" :disabled="isDisabled">
          Agregar Nota
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      nuevoTitulo: "",
      nuevaDescripcion: "",
    };
  },
  methods: {
    cerrarModal: function () {
      this.$emit("cerrarModalAgregar");
    },
    agregarNota: function () {
      let nuevaNota = {
        id: Date.now(),
        fecha: this.fechaLetrada,
        titulo: this.nuevoTitulo,
        descripcion: this.nuevaDescripcion,
        estado: false,
      };
      this.$emit("agregarNota", nuevaNota);
    },
  },
  computed: {
    isDisabled: function () {
      return this.nuevoTitulo.length && this.nuevaDescripcion.length
        ? false
        : true;
    },
    fechaLetrada: function () {
      const tiempo = Date.now();
      const hoy = new Date(tiempo);
      return hoy.toLocaleDateString();
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