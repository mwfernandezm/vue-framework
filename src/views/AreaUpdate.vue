<template>
  <h1>Areas Editar</h1>
  <form @submit.prevent="update()">
    <p>Nombre: <input type="text" v-model="payload.nombre" required /></p>
    <p>Encargado: <input type="text" v-model="payload.encargado" required /></p>
    <p>
      Cant Funcionarios:
      <input type="text" v-model="payload.cantFuncionarios" required />
    </p>
    <button type="submit">Actualizar</button>
  </form>
</template>
<script>
export default {
  name: "AreaView",
  data() {
    const api = process.env.VUE_APP_API;
    return {
      api,
      items: [],
      payload: {
        nombre: null,
        encargado: null,
        cantFuncionarios: null,
      },
    };
  },
  methods: {
    getOne() {
      this.axios({
        method: "get",
        url: this.api + "/areas/" + this.$route.params.id,
      })
        .then((response) => {
          this.payload = response.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    update() {
      if (confirm("Esta seguro de actualizar el registro?")) {
        this.axios({
          method: "patch",
          url: this.api + "/areas/" + this.$route.params.id,
          data: this.payload,
        })
          .then((response) => {
            console.log(response);
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
  },
  components: {},

  mounted() {
    this.getOne();
  },
};
</script>
