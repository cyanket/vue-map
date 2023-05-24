<template>
  <div id="form-container">
    <form id="location-form" @submit.prevent="emitLocation">
      <h2>Map the Address</h2>
      <input v-model="name" type="text" placeholder="Location name">
      <textarea v-model="description" placeholder="Description"></textarea>
      <input v-model="lat" type="number" @keypress="latVal" step="0.01" placeholder="Latitude">
      <input v-model="lon" type="number" @keypress="lonVal" step="0.01" placeholder="Longitude">
      <button class="disabled-btn" v-if="!isValid" disabled>Add Location</button>
      <button class="add-btn" v-else>Add Location</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'FormComponent',
  data() {
    return {
      name: '',
      description: '',
      lat: '',
      lon: ''
    }
  },
  computed: {
    isValid() {
      return this.name && this.description && this.lat && this.lon;
    }
  },
  methods: {
    latVal($event) {
      let keyCode = ($event.keyCode ? $event.keyCode : $event.which);
      if (keyCode === 45 && this.lat.indexOf('-') === -1 && this.lat.length === 0) {
        return;
      }
      if ((keyCode < 48 || keyCode > 57) && (keyCode !== 46 || this.lat.indexOf('.') != -1)) {
        $event.preventDefault();
      }
      if(this.lat!=null && this.lat.indexOf(".")>-1 && (this.lat.split('.')[1].length > 1)){
      $event.preventDefault();
      }
    },
    lonVal($event) {
      let keyCode = ($event.keyCode ? $event.keyCode : $event.which);
      if (keyCode === 45 && this.lon.indexOf('-') === -1 && this.lon.length === 0) {
        return;
      }
      if ((keyCode < 48 || keyCode > 57) && (keyCode !== 46 || this.lon.indexOf('.') != -1)) {
        $event.preventDefault();
      }
      if(this.lon!=null && this.lon.indexOf(".")>-1 && (this.lon.split('.')[1].length > 1)){
      $event.preventDefault();
      }
    },
    emitLocation() {
      if (this.isValid) {
        const newLocation = {
          id: Date.now(),
          name: this.name,
          description: this.description,
          lat: parseFloat(this.lat),
          lon: parseFloat(this.lon)
        };

        this.$emit('addLocation', newLocation);
        this.name = '';
        this.description = '';
        this.lat = '';
        this.lon = '';
      }
    },
  }
}
</script>

<style scoped>
#form-container {
  flex: 1;
  padding: 20px;
  background-color: #8082ff30;
}

#location-form {
  max-width: 300px;
  margin: auto;
  text-align: center;
}

#location-form h2 {
  color: #9325d3;
  margin-bottom: 20px;
}

#location-form input, #location-form textarea {
  display: block;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 8px;
  margin-bottom: 10px;
  box-sizing: border-box;
}

#location-form textarea {
  height: 120px;
  max-width: 300px;
}

.add-btn {
  display: block;
  width: 40%;
  margin: auto;
  padding: 10px;
  border: none;
  border-radius: 1rem;
  background-color: #9325d3;
  color: white;
  cursor: pointer;
}

.disabled-btn {
  display: block;
  width: 40%;
  margin: auto;
  padding: 10px;
  border: none;
  border-radius: 1rem;
  background-color: #9325d375;
  color: white;
  cursor: not-allowed;
}
</style>
