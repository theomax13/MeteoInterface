<template>
  <div class="d-flex flex-wrap justify-content-center">
    <div
      class="card m-3"
      style="width: 18rem; height: 16rem"
      v-for="sensor in liveSensorData"
      :key="sensor.id"
    >
      <div class="card-body">
        <!-- <h5>{{ sensor }}</h5> -->
        <h5
          class="card-title"
          v-if="sensor.nom_capteur !== null && sensor.nom_capteur !== undefined"
        >
          Données de {{ sensor.nom_capteur }}
        </h5>
        <h5 class="card-title" v-else>
          Données du Capteur de Température n°{{ sensor.capteur }}
        </h5>
        <p class="card-text">Date: {{ sensor.received_at }}</p>
        <p class="card-text">Température: {{ sensor.temperature_relevee }}°C</p>
        <p class="card-text" v-if="sensor.niveau_humidite != 255">
          Humidité: {{ sensor.niveau_humidite }} %
        </p>
        <p class="card-text" v-if="sensor.niveau_humidite == 255">
          Humidité: Non disponible
        </p>
        <!-- <button
          type="button"
          class="btn btn-success"
          data-bs-toggle="modal"
          data-bs-target="#modalModifNames"
          @click="openModal(sensor)"
        >
          Modifier
        </button> -->
      </div>
    </div>
  </div>

  <ModalComponent
    :sensorNames="sensorNames"
    @updateSensorName="updateSensorName"
  />
</template>

<script>
import ModalComponent from "./ModalComponent.vue";

export default {
  name: "CardComponent",
  props: {
    liveSensorData: Array,
    sensorNames: Array,
  },
  component: {
    ModalComponent,
  },
  methods: {
    modalId(capteur) {
      this.$emit("modalId", capteur);
    },
  },
};
</script>
