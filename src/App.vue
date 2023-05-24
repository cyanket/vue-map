<template>
  <div id="app">
    <FormComponent @addLocation="addLocation" />
      <MapComponent :locations="locations" />
      <button id="search-toggle" @click="showSearch = !showSearch">
        <i class="fas fa-search" style="font-size:16px"></i>
      </button>
      <div v-if="showSearch" id="search-container">
        <input v-model="search" type="text" placeholder="Search locations" @input="performSearch">
        <div v-if="searchResults.length" id="search-results">
          <div v-for="result in searchResults" :key="result.place_id" @click="goToSearchResult(result)">
            {{ result.display_name }}
          </div>
        </div>
      </div>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.1/css/all.min.css" rel="stylesheet">
  </div>
</template>

<script>
import FormComponent from './FormComponent.vue';
import MapComponent from './MapComponent.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    FormComponent,
    MapComponent
  },
  data() {
    return {
      locations: [],
      search: '',
      searchResults: [],
      showSearch: false,
    }
  },
  methods: {
    addLocation(newLocation) {
      this.locations.push(newLocation);
    },
    performSearch() {
      if (this.search.length >= 2) {
        axios.get(`https://nominatim.openstreetmap.org/search?format=json&q=${this.search}`)
        .then(response => {
          this.searchResults = response.data;
        });
      } else {
        this.searchResults = [];
      }
    },
    goToSearchResult(result) {
      this.locations.push({
        id: Date.now(),
        name: result.display_name,
        description: '',
        lat: parseFloat(result.lat),
        lon: parseFloat(result.lon)
      });
      this.center = [parseFloat(result.lat), parseFloat(result.lon)];
      this.zoom = 10;
      this.searchResults = [];
      this.search = '';
    }
  }
}
</script>

<style scoped>
#app {
  display: flex;
  height: 97vh;
  @media screen and (max-width: 400px) {
    display: block;
  }
}

#search-toggle {
  position: absolute;
  top: 16px;
  right: 16px;
  background-color: white;
  padding: 8px;
  border-radius: 16%;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.15);
  cursor: pointer;
  border: none;
  z-index: 10000;
  @media screen and (max-width: 400px) {
    top: 53%;
  }
}

#search-toggle:hover {
  background-color: #e5e8e8;
}

#search-container {
  position: absolute;
  top: 16px;
  right: 60px;
  background-color: white;
  padding: 10px;
  border-radius: 4px;
  width: 200px;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.15);
  z-index: 10000;
  @media screen and (max-width: 400px) {
    top: 53%;
  }
}

#search-container input {
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

#search-results {
  margin-top: 10px;
  max-height: 50vh;
  overflow: auto;
  @media screen and (max-width: 400px) {
    max-height: 35vh;
  }
}

#search-results::-webkit-scrollbar {
  display: none;
}

#search-results div {
  padding: 5px;
  border-bottom: 1px solid #ccc;
  cursor: pointer;
}

#search-results div:hover {
  background-color: #f9f9f9;
}
</style>
