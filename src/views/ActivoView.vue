<template>
  <div class="container">
    <h5>Activos</h5>
    <div class="card">
      <div class="card-content">
        <form @submit.prevent="nuevo()">
          <h5>Nuevo Activo</h5>
          <p>Tipo: <input type="text" v-model="payload.tipo" required /></p>
          <p>Marca: <input type="text" v-model="payload.marca" required /></p>
          <p>Modelo: <input type="text" v-model="payload.modelo" required /></p>
          <p>
            Estado (nuevo o usado o desuso):
            <input type="text" v-model="payload.estado" required />
          </p>
          <p>Area</p>
          <p v-if="areas.length > 0">
            <select v-model="payload.areaId">
              <option :value="area.id" v-for="area in areas">
                {{ area.nombre }}
              </option>
            </select>
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
          <h5>Buscar activo</h5>
          <p>
            Tipo activo:
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
          <select @change="filter('estado', $event.target.value)">
            <option value="" selected>todos</option>
            <option value="nuevo">nuevo</option>
            <option value="usado">usado</option>
            <option value="desuso">desuso</option>
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
              <th>Tipo</th>
              <th>Marca</th>
              <th>Modelo</th>
              <th>Estado</th>
              <th>Area Id</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in items" :key="index">
              <td>{{ item.id }}</td>
              <td>{{ item.tipo }}</td>
              <td>{{ item.marca }}</td>
              <td>{{ item.modelo }}</td>
              <td>{{ item.estado }}</td>
              <td>{{ item.areaId }}</td>
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
  name: "ActivoView",
  data() {
    const api = process.env.VUE_APP_API;
    return {
      api,
      items: [],
      search: "",
      toFilter: "",
      areas: [],
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
    filter(estado, value) {
      this.toFilter = value === "" ? "" : "&" + estado + "=" + value;
      this.getList();
    },
    update(id) {
      this.$router.push("/activo/" + id);
    },
    eliminar(id) {
      if (confirm("Esta seguro de eliminar?")) {
        this.axios({
          method: "delete",
          url: this.api + "/activos/" + id,
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
        url: this.api + "/activos",
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
        url: this.api + "/activos?q=" + this.search + this.toFilter,
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

    getAreaList() {
      this.axios({
        method: "get",
        url: this.api + "/areas",
      })
        .then((response) => {
          this.areas = response.data;
          const intervalo = setTimeout(() => {
            this.intSelect();
            clearTimeout(intervalo);
          }, 3000);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    intSelect() {
      this.getList();
      this.getAreaList();
      var elems = document.querySelectorAll("select");
      var instances = M.FormSelect.init(elems);
      console.log(instances);
    },
  },
  components: {},
  mounted() {
    this.getList();
    this.getAreaList();
    var elems = document.querySelectorAll("select");
    var instances = M.FormSelect.init(elems);
    console.log(instances);
  },
};
</script>
