<template>
  <div class="container">
    <h5>Areas Editar</h5>
    <div class="card">
      <div class="card-content">
        <form @submit.prevent="update()">
          <p>Nombre: <input type="text" v-model="payload.nombre" required /></p>
          <p>
            Encargado:
            <input type="text" v-model="payload.encargado" required />
          </p>
          <p>
            Cant Funcionario:
            <input type="text" v-model="payload.cantFuncionario" required />
          </p>
          <p>
            <label>
              <input
                type="checkbox"
                class="filled-in"
                v-model="payload.active"
              />
              <span>Activo</span>
            </label>
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
  name: "AreaView",
  data() {
    const api = process.env.VUE_APP_API;
    return {
      api,
      items: [],
      payload: {
        nombre: null,
        encargado: null,
        cantFuncionario: null,
        active: null,
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
