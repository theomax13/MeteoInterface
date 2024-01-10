<template>
  <CardComponent :liveSensorData="liveSensorData" :sensorNames="sensorNames" />

  <ChartComponent :sensorData="sensorData" />
</template>

<script>
import ChartComponent from "./ChartComponent.vue";

import CardComponent from "./CardComponent.vue";

export default {
  name: "LiveDisplay",
  props: {
    liveSensorData: {
      required: true,
    },
    sensorData: {
      required: true,
    },
    sensorNames: {
      required: true,
    },
  },
  components: {
    ChartComponent,
    CardComponent,
  },
};
</script>
