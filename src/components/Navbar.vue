<template>
  <nav class="navbar bg-light">
    <div class="container-fluid">
      <a class="navbar-brand"><b>{{ title }}</b></a>
      <form class="d-flex" role="search">
        <div class="form-check form-switch">
          <label class="form-check-label" for="flexSwitchCheckDefault">
            <span class="material-symbols-outlined">
              dark_mode
            </span>
          </label>
          <input class="form-check-input" type="checkbox" role="switch" id="flexSwitchCheckDefault">
        </div>
        <input class="form-control me-2" type="search" id="search" :placeholder="$t('searchPlaceholder')" aria-label="Search">
        <div class="btn btn-outline-success" @click="search()">{{$t('searchButton')}}</div>
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