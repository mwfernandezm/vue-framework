<template>
  <h1>Areas</h1>

  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Nombre</th>
        <th>Encargado</th>
        <th>Cantdad Funcionarios</th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="(item, index) in items" :key="index">
        <td>{{ item.id }}</td>
        <td>{{ item.nombre }}</td>
        <td>{{ item.encargado }}</td>
        <td>{{ item.cantFuncionarios }}</td>
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
    };
  },
  methods: {
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
