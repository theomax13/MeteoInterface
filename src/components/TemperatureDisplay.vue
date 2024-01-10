<!-- TemperatureDisplay.vue -->
<template>
  <div class="temperature-display d-flex flex-wrap justify-content-center">
    <div class="form-check form-check-inline">
      <input
        class="form-check-input"
        type="radio"
        name="radioGroup"
        id="radioTable"
        value="table"
        v-model="switchButton"
        checked
      />
      <label class="form-check-label" for="flexRadioDefault1"> Tableau </label>
    </div>
    <div class="form-check form-check-inline">
      <input
        class="form-check-input"
        type="radio"
        name="radioGroup"
        id="radioCards"
        value="card"
        v-model="switchButton"
      />
      <label class="form-check-label" for="flexRadioDefault2"> Cards </label>
    </div>
    <select
      class="form-select m-3"
      aria-label="selectTaille"
      v-model="tailleHistorique"
    >
      <option value="">All</option>
      <option value="10">10</option>
      <option value="20">20</option>
      <option value="30">30</option>
      <option value="50">50</option>
      <option value="100">100</option>
    </select>
    <select
      class="form-select m-3"
      aria-label="selectCapteur"
      v-model="selectCaptor"
    >
      <option value="">All</option>
      <option v-for="names in sensorNames" :key="names.capteur">
        <p v-if="names.nom_capteur">{{ names.nom_capteur }}</p>
        <p v-else>{{ names.capteur }}</p>
      </option>
    </select>

    <CardComponent v-if="switchButton === 'card'" :sensorData="filteredData" />

    <TableComponent
      v-if="switchButton === 'table'"
      :sensorData="filteredData"
    />

    <Moda
      :sensorData="sensorData"
      :sensorNames="sensorNames"
      @updateSensorName="updateSensorName"
    />
  </div>
</template>

<script>
import CardComponent from "@/components/CardComponent.vue";
import TableComponent from "./TableComponent.vue";
import Moda from "./ModalComponent.vue";

export default {
  props: {
    sensorData: Array,
    sensorNames: Array,
  },
  components: {
    CardComponent,
    TableComponent,
    Moda,
  },
  data() {
    return {
      switchButton: "table",
      tailleHistorique: "",
      selectCaptor: "",
    };
  },
  computed: {
    filteredData() {
      let filteredByCaptor = [];

      // Filtrer par nom de capteur
      if (this.selectCaptor !== "") {
        filteredByCaptor = this.sensorData.filter((sensor) => {
          return (
            sensor.nom_capteur &&
            sensor.nom_capteur.toString() === this.selectCaptor
          );
        });
      } else {
        filteredByCaptor = this.sensorData;
      }

      // Limiter la taille de la liste
      if (this.tailleHistorique === "") {
        return filteredByCaptor;
      } else {
        return filteredByCaptor.slice(0, this.tailleHistorique);
      }
    },
  },
  watch: {},
};
</script>

<style scoped></style>
