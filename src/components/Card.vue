<template>
  <div class="card">
    <h1>{{ titulo }}</h1>
    <h5>{{ fecha }}</h5>
    <p>{{ descripcion }}</p>
    <div class="buttons">
      <span @click="editarNota"
        ><img src="@/assets/edit.png" alt="edit"
      /></span>
      <span @click="eliminarNota"
        ><img src="@/assets/delete.png" alt="delete"
      /></span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    titulo: String,
    descripcion: String,
    fecha: String,
    estado: Boolean,
    id: Number,
  },
  methods: {
    eliminarNota: function () {
      this.$emit("eliminarNota", this.id);
    },
    editarNota: function () {
      this.$emit("editarNota", this.titulo, this.descripcion, this.id);
    },
  },
};
</script>

<style  scoped>
.card {
  border: #fff 1px solid;
  /* border-radius: 15px; */
  padding: 1.5em;
  margin-bottom: 2em;
  break-inside: avoid;
  position: relative;
}
@media (max-width: 680px) {
  .card {
    margin-bottom: 1em;
  }
}
h1,
h5,
p {
  color: #fff;
}
h5 {
  padding: 0.2em 0 0.4em 0;
}
.buttons {
  position: absolute;
  top: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  visibility: hidden;
}
.card:hover .buttons {
  visibility: visible;
}
.buttons span {
  box-shadow: inset 0px 0px 15px 3px #23395e;
  background: linear-gradient(to bottom, #2e466e 5%, #415989 100%);
  background-color: #2e466e;
  /* border-radius: 50%; */
  border: 1px solid #1f2f47;
  display: inline-block;
  cursor: pointer;
  font-size: 0.5em;
  padding: 0.8em;
  border: #fff solid 2px;
  margin: 0.5em;
}
img {
  filter: invert(1);
  height: 2em;
}
</style>