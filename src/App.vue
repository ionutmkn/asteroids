<template>
  <div id="app">
    <AsteroidGrid @remove="remove" :asteroids="asteroids" header="Near-Earth Asteroids"></AsteroidGrid>
  </div>
</template>

<script>

import AsteroidGrid from "./components/AsteroidGrid.vue";

import axios from "axios";
export default {
  name: "app",
  components: {
    AsteroidGrid
  },
  data() {
    return {
      asteroids: []
    };
  },
  created: function() {
    this.fetchAsteroids();

  },
  methods: {
    fetchAsteroids: function() {
      var apiKey = "Y437uIoUfia17J5XjH1ycaxdrirmmyfMKHmZUj3d";
      var url = "https://api.nasa.gov/neo/rest/v1/neo/browse?api_key=" + apiKey;
      axios.get(url).then(res => {
        for (let i = 0; i < res.data.near_earth_objects.length; i++) {
          if (res.data.near_earth_objects[i].close_approach_data.length) {
            this.asteroids.push(res.data.near_earth_objects[i]);
          }
        }

        this.asteroids = this.asteroids.slice(0, 10);
      });
    },
    remove: function(index) {
      this.asteroids.splice(index, 1);
    }
  }
};
</script>

<style>
[v-cloak] {
  display: none;
}
</style>
