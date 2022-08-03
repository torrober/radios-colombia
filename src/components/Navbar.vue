<template>
<nav class="navbar bg-light">
  <div class="container-fluid">
    <a class="navbar-brand"><b>{{title}}</b></a>
    <form class="d-flex" role="search">
      <input class="form-control me-2" type="search" id="search" placeholder="Buscar radios.." aria-label="Search">
      <div class="btn btn-outline-success" @click="search()">Buscar</div>
    </form>
  </div>
</nav>
</template>

<script>
export default {
  name: 'Navbar',
  props: {
    title: String
  },
  methods: {
    search() {
      const value = document.getElementById('search').value;
      this.$emit('searchStation', value)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->