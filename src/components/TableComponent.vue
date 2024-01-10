<template>
  <table class="table table-striped table-hover">
    <thead>
      <tr>
        <th scope="col">Capteur</th>
        <th scope="col">Id capteur</th>
        <th scope="col">Date</th>
        <th scope="col">Température</th>
        <th scope="col">Humidité</th>
        <th scope="col">Batterie</th>
        <th scope="col">Force du signal</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="sensor in sensorData" :key="sensor[0]">
        <th scope="row">{{ sensor.nom_capteur }}</th>
        <td>{{ sensor.capteur }}</td>
        <td>{{ sensor.received_at }}</td>
        <td>{{ sensor.temperature_relevee }}°C</td>
        <td v-if="sensor.niveau_humidite != 255">
          {{ sensor.niveau_humidite }} %
        </td>
        <td v-if="sensor.niveau_humidite == 255">Non disponible</td>
        <td>{{ sensor.niveau_batterie }}</td>
        <td>{{ sensor.force_signal }}</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: "TableComponent",
  props: {
    sensorData: {
      required: true,
    },
  },
};
</script>
