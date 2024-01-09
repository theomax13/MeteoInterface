<!-- App.vue -->
<template>
  <div id="app">
    <TemperatureDisplay
      :sensorData="sensorData"
      @updateSensorName="updateSensorName"
    />
  </div>
</template>

<script>
import TemperatureDisplay from "@/components/TemperatureDisplay.vue";
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      sensorData: [],
    };
  },
  components: {
    TemperatureDisplay,
  },
  mounted() {
    this.fetchSensorData();
  },
  methods: {
    async fetchSensorData() {
      try {
        const response = await axios.get("https://api-meteo.ykpf.net");
        this.sensorData = Object.values(response.data);
        console.log(this.sensorData);
      } catch (error) {
        console.error("Error fetching sensor data:", error);
      }
    },
    // updateSensorName({ id, name }) {
    //   const sensor = this.sensorData.find((sensor) => sensor.id === id);
    //   if (sensor) {
    //     sensor.nom_capteur = name;
    //   }
    // },
    async updateSensorName(capteur) {
      console.log(capteur);
      const url = `https://api-meteo.ykpf.net/capteur/${capteur.capteur}`;
      const data = {
        nom: capteur.name,
      };

      try {
        const response = await axios.put(url, data);
        console.log(response.data);
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
/* Ajoutez des styles CSS au besoin */
</style>

<!--pour le put :  /captor/{idducaptor} -->
