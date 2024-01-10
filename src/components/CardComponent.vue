<template v-for="sensor in sensorData" :key="sensor[0]">
  <div class="card m-3" style="width: 18rem; height: 16rem">
    <div class="card-body">
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
      <button
        type="button"
        class="btn btn-success"
        data-bs-toggle="modal"
        data-bs-target="#modalModifNames"
        @click="modalId(sensor.capteur)"
      >
        Modifier
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardComponent",
  props: {
    sensorData: {
      required: true,
    },
  },
  methods: {
    modalId(capteur) {
      this.$emit("modalId", capteur);
    },
  },
};
</script>
