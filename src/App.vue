<template>
  <Navbar title="Radios Colombia" @searchStation="searchSt($event)" />
  <div class="container-fluid" style="padding-bottom: 10%;">
    <div v-if="loaded" class="row">
      <div v-if="isSearching" @click="clearSearch()" class="p-5 bg-primary text-white rounded">
        <button type="button" class="btn btn-dark">🡠 Regresar</button>
        <h1 id="searchRes">Resultados de: {{sq}}</h1>
      </div>
      <div v-for="element in content" v-bind:key="element" class="col-12">
        <Card @changeStation="playStation($event)" :tags="element.tags" :name="element.name" :state="element.state"
          :url="element.url_resolved" :cover="element.favicon"></Card>
      </div>
    </div>
  </div>
  <Player v-if="hasRadioOpen" :name="currentStation" :state="currentStationState" :url="currentStationUrl" :cover="currentStationCover" />
</template>

<script>
import Navbar from './components/Navbar.vue';
import Card from './components/Card.vue';
import Player from './components/Player.vue';
const apiUrl = "https://de1.api.radio-browser.info/json/stations/bycountrycodeexact/CO";
export default {
  name: 'App',
  data() {
    return {
      loaded: false,
      search: false,
      content: [],
      auxArray: [],
      sq: "",
      isSearching: false,
      hasRadioOpen: false,
      currentStation: "",
      currentStationState: "",
      currentStationCover: "",
      currentStationUrl: ""
    }
  },
  components: {
    Navbar,
    Card,
    Player
  },
  methods: {
    loadData() {
      fetch(apiUrl)
        .then(res => res.json())
        .then(res => { this.content = res; this.auxArray = res; this.loaded = true; })
    },
    playStation(data) {
      const station = JSON.parse(data);
      this.hasRadioOpen = true;
      this.currentStation = station.name;
      this.currentStationState = station.state;
      this.currentStationUrl = station.url;
      this.currentStationCover = station.cover;
    },
    searchSt(keyword) {
      if (keyword.length > 0) {
        this.isSearching = true;
        this.content = [];
        for (let i = 0; i < this.auxArray.length; i++) {
          if (this.auxArray[i].name.toLowerCase().includes(keyword.toLowerCase()) || this.auxArray[i].state.toLowerCase().includes(keyword.toLowerCase()) || this.auxArray[i].tags.toLowerCase().includes(keyword.toLowerCase())) {
            this.content.push(this.auxArray[i])
          }
        }
        this.sq = keyword;
      } else {
        this.isSearching = false;
        this.content = this.auxArray;
        this.sq = keyword;
      }
    },
    clearSearch() {
      this.content = this.auxArray;
      this.isSearching = false;
      document.querySelector("#search").value= "";
    }
  },
  mounted() {
    this.loadData()
  }
}
</script>
