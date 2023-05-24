<template>
  <div id="app-map">
    <l-map ref="map" :zoom="zoom" :center="center">
      <l-tile-layer :url="url"></l-tile-layer>
      <l-marker v-for="loc in locations" :key="loc.id" :lat-lng="{lat: loc.lat, lon: loc.lon}">
        <l-popup>{{ loc.name }}<br>{{ loc.description }}</l-popup>
      </l-marker>
    </l-map>
  </div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup } from 'vue2-leaflet';
import 'leaflet/dist/leaflet.css';

export default {
  name: 'MapComponent',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
  },
  props: {
    locations: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      search: '',
      searchResults: [],
      showSearch: false,
      zoom: 5,
      center: [0, 100],
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
    }
  },
  watch: {
    locations: function() {
      let latestLocation = this.locations[this.locations.length - 1];
      this.$emit('update:locations', this.locations);
      this.center = [latestLocation.lat, latestLocation.lon];
    }
  }
}
</script>

<style scoped>
#app-map {
  position: relative;
  flex: 2;
  @media screen and (max-width: 400px) {
    height: 47vh;
  }
}
</style>
