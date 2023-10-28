<template>
  <h1>Areas</h1>
  <form @submit.prevent="nuevo()">
    <p>Nombre: <input type="text" v-model="payload.nombre" required /></p>
    <p>Encargado: <input type="text" v-model="payload.encargado" required /></p>
    <p>
      Cantidad Funcionarios:
      <input type="text" v-model="payload.cantFuncionarios" required />
    </p>
    <button type="submit">Agregar</button>
  </form>
  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Nombre</th>
        <th>Encargado</th>
        <th>Cantidad Funcionarios</th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="(item, index) in items" :key="index">
        <td>{{ item.id }}</td>
        <td>{{ item.nombre }}</td>
        <td>{{ item.encargado }}</td>
        <td>{{ item.cantFuncionarios }}</td>
        <a class="btn-floating btn-large waves-effect waves-light red"
          ><i class="material-icons" @click="eliminar(item.id)">delete</i></a
        >
        <a class="btn-floating btn-large waves-effect waves-light blue"
          ><i class="material-icons" @click="update(item.id)">edit</i></a
        >
      </tr>
    </tbody>
  </table>
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
    update(id) {
      this.$router.push("/area/" + id);
    },
    eliminar(id) {
      if (confirm("Esta seguro de eliminar?")) {
        this.axios({
          method: "delete",
          url: this.api + "/areas/" + id,
        })
          .then((response) => {
            console.log(response.data);
            this.getList();
          })
          .catch((error) => {
            console.log(error);
          });
      }
    },
    nuevo() {
      this.axios({
        method: "post",
        url: this.api + "/areas",
        data: this.payload,
      })
        .then((response) => {
          this.getList();
          console.log(response.data);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getList() {
      this.axios({
        method: "get",
        url: this.api + "/areas",
      })
        .then((response) => {
          console.log(response.data);
          this.items = response.data;
          console.log(this.items);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  components: {},
  mounted() {
    this.getList();
  },
};
</script>
