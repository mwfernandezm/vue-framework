<template>
  <nav>
    <div class="nav-wrapper">
      <a href="/" class="brand-logo right">F&F App</a>
      <ul id="nav-mobile" class="left">
        <li>
          <a class="waves-effect waves-light">
            <i class="material-icons" @click="menu()">menu</i>
          </a>
        </li>
        <li>
          <!-- <a href="#" data-target="mobile-demo"><i class="material-icons">menu</i></a> -->
        </li>
      </ul>
    </div>
  </nav>

  <ul class="sidenav" id="mobile-demo">
    <li><router-link to="/">Inicio</router-link></li>
    <li><router-link to="/about">Nosotros</router-link></li>
    <li><router-link to="/area">Area</router-link></li>
    <li><router-link to="/activo">Activo</router-link></li>
  </ul>
</template>

<script>
import M from "materialize-css";
export default {
  name: "MyMenu",
  data() {
    return {
      sidenav: null,
    };
  },
  props: [],
  methods: {
    menu() {
      this.sidenav.open();
    },
  },
  mounted() {
    var elems = document.querySelectorAll(".sidenav");
    var instances = M.Sidenav.init(elems);
    this.sidenav = instances[0];
  },
};
</script>

<style></style>
