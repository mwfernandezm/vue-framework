<template>
  <div class="container">
    <h5>Areas</h5>
    <div class="card">
      <div class="card-content">
        <form @submit.prevent="nuevo()">
          <h5>Nueva area</h5>
          <p>Nombre: <input type="text" v-model="payload.nombre" required /></p>
          <p>
            Encargado:
            <input type="text" v-model="payload.encargado" required />
          </p>
          <p>
            Cantidad Funcionario:
            <input type="text" v-model="payload.cantFuncionario" required />
          </p>
          <!--<p>
            Estado:
            <input type="text" v-model="payload.active" required />
          </p>-->

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
            Agregar
          </button>
        </form>
      </div>
    </div>
    <div class="card">
      <div class="card-content">
        <form @submit.prevent="getList()">
          <h5>Buscar area</h5>
          <p>
            Nombre area:
            <input type="search" v-model="search" @search="getList()" />
          </p>
          <button type="submit" class="waves-effect waves-light btn-small">
            Buscar
          </button>
        </form>
      </div>
    </div>
    <div class="card">
      <div class="card-content">
        <h5>Filtros</h5>
        <div class="input-field">
          <select @change="filter('active', $event.target.value)">
            <option value="" selected>todos</option>
            <option value="true">activo</option>
            <option value="false">inactivo</option>
          </select>
          <label>Materialize Select</label>
        </div>
      </div>
    </div>
    <div class="card">
      <div class="card-content">
        <table>
          <thead>
            <tr>
              <th>Id</th>
              <th>Nombre</th>
              <th>Encargado</th>
              <th>Cantidad Funcionario</th>
              <th>Estado</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in items" :key="index">
              <td>{{ item.id }}</td>
              <td>{{ item.nombre }}</td>
              <td>{{ item.encargado }}</td>
              <td>{{ item.cantFuncionario }}</td>
              <td>{{ item.active }}</td>
              <a
                class="app-btn btn-small btn-floating btn-large waves-effect waves-light red"
                ><i class="material-icons" @click="eliminar(item.id)"
                  >delete</i
                ></a
              >
              <a
                class="app-btn btn-small btn-floating btn-large waves-effect waves-light blue"
                ><i class="material-icons" @click="update(item.id)">edit</i></a
              >
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>
<script>
import M from "materialize-css";
export default {
  name: "AreaView",
  data() {
    const api = process.env.VUE_APP_API;
    return {
      api,
      items: [],
      search: "",
      toFilter: "",
      payload: {
        nombre: null,
        encargado: null,
        cantFuncionario: null,
        active: null,
      },
    };
  },
  methods: {
    filter(nombre, value) {
      this.toFilter = value === "" ? "" : "&" + nombre + "=" + value;
      this.getList();
    },
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
    getList(nombre, value) {
      this.axios({
        method: "get",
        url: this.api + "/areas?q=" + this.search + this.toFilter,
      })
        .then((response) => {
          console.log(response.data);
          this.items = response.data;
          console.log(this.items);
          console.log(nombre);
          console.log(value);
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  components: {},
  mounted() {
    this.getList();
    var elems = document.querySelectorAll("select");
    var instances = M.FormSelect.init(elems);
    console.log(instances);
  },
};
</script>
