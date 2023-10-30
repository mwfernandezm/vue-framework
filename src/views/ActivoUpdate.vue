<template>
  <div class="container">
    <h5>Activos Editar</h5>
    <div class="card">
      <div class="card-content">
        <form @submit.prevent="update()">
          <p>Tipo: <input type="text" v-model="payload.tipo" required /></p>
          <p>Marca: <input type="text" v-model="payload.marca" required /></p>
          <p>Modelo: <input type="text" v-model="payload.modelo" required /></p>
          <p>Estado: <input type="text" v-model="payload.estado" required /></p>
          <p>
            Area Id: <input type="text" v-model="payload.areaId" required />
          </p>
          <button type="submit" class="waves-effect waves-light btn-small">
            Editar
          </button>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "ActuvoView",
  data() {
    const api = process.env.VUE_APP_API;
    return {
      api,
      items: [],
      payload: {
        tipo: null,
        marca: null,
        modelo: null,
        estado: null,
        areaId: null,
      },
    };
  },
  methods: {
    getOne() {
      this.axios({
        method: "get",
        url: this.api + "/activos/" + this.$route.params.id,
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
          url: this.api + "/activos/" + this.$route.params.id,
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
