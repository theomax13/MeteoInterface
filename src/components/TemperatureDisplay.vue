<!-- TemperatureDisplay.vue -->
<template>
  <div class="temperature-display">
    <div class="sensor-item" v-for="sensor in sensorData" :key="sensor[0]">
      <h2 v-if="sensor.nom_capteur">
        <span>Données de {{ sensor.nom_capteur }}</span>
        <button @click="openModifyDialog(sensor)">Modifier</button>
      </h2>
      <h2 v-else>
        <span>Données du Capteur de Température n°{{ sensor.capteur }}</span>
        <button @click="openModifyDialog(sensor)">Modifier</button>
      </h2>
      <div class="data-item">
        <span>Date:</span>
        <span>{{ sensor.received_at }}</span>
      </div>
      <div class="data-item">
        <span>Température:</span>
        <span>{{ sensor.temperature_relevee }} °C</span>
      </div>
      <div class="data-item" v-if="sensor.niveau_humidite != 255">
        <span>Humidité:</span>
        <span>{{ sensor.niveau_humidite }} %</span>
      </div>
      <div class="data-item" v-if="sensor.niveau_humidite == 255">
        <span>Humidité:</span>
        <span>Non disponible</span>
      </div>
    </div>

    <!-- Modal de modification du nom du capteur -->
    <div v-if="showModifyDialog" class="modal">
      <div class="modal-content">
        <h2>Modifier le nom du capteur</h2>
        <label for="newSensorName">Nouveau nom:</label>
        <input v-model="newSensorName" id="newSensorName" />
        <button @click="saveModifiedName">Enregistrer</button>
        <button @click="closeModifyDialog">Annuler</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    sensorData: Array,
  },
  data() {
    return {
      newSensorName: "",
      showModifyDialog: false,
      selectedSensor: null,
    };
  },
  methods: {
    openModifyDialog(sensor) {
      this.selectedSensor = sensor;
      this.newSensorName = sensor.nom_capteur || "";
      this.showModifyDialog = true;
    },
    closeModifyDialog() {
      this.showModifyDialog = false;
    },
    saveModifiedName() {
      if (this.selectedSensor) {
        this.$emit("updateSensorName", {
          capteur: this.selectedSensor.capteur,
          name: this.newSensorName,
        });
        this.showModifyDialog = false;
      }
    },
  },
};
</script>

<style scoped>
.temperature-display {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  max-width: 1200px;
  margin: auto;
}

.sensor-item {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.data-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.data-item span {
  font-weight: bold;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: #fff;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
</style>
