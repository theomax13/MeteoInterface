<!-- App.vue -->
<template>
  <div id="app">
    <NavBarComponent @switchDisplay="switchDisplay" />
    <TemperatureDisplay
      v-if="historiqueDisplay"
      :sensorData="sensorData"
      :sensorNames="sensorNames"
    />
    <KeepAlive v-else>
      <LiveDisplay
        :liveSensorData="liveSensorData"
        :sensorData="sensorData"
        :sensorNames="sensorNames"
        @updateSensorName="updateSensorName"
      />
    </KeepAlive>
  </div>
</template>

<script>
import TemperatureDisplay from "@/components/TemperatureDisplay.vue";
import NavBarComponent from "@/components/NavBarComponent.vue";
import LiveDisplay from "@/components/LiveDisplayComponent.vue";

import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      historiqueDisplay: false,
      sensorData: [],
      sensorNames: [],
      liveSensorData: [],
    };
  },
  components: {
    TemperatureDisplay,
    NavBarComponent,
    LiveDisplay,
  },
  mounted() {
    this.fetchSensorData();
  },
  methods: {
    async fetchSensorData() {
      try {
        const response = await axios.get("https://api-meteo.ykpf.net");
        const responseLive = await axios.get("https://api-meteo.ykpf.net/live");
        const responseNames = await axios.get(
          "https://api-meteo.ykpf.net/capteurs"
        );
        this.sensorData = Object.values(response.data);
        this.liveSensorData = Object.values(responseLive.data);
        this.sensorNames = Object.values(responseNames.data);
        this.sensorData.pop();
        this.liveSensorData.pop();
        this.sensorNames.pop();
      } catch (error) {
        console.error("Error fetching sensor data:", error);
      }
    },
    async updateSensorName(capteur) {
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
    switchDisplay() {
      this.historiqueDisplay = !this.historiqueDisplay;
    },
  },
};
</script>

<style>
/* Ajoutez des styles CSS au besoin */
</style>
