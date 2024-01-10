<template>
  <div id="chart">
    <apexcharts v-if="ready" type="line" height="350" :options="chartOptions" :series="series"></apexcharts>
  </div>
</template>

<script>
import VueApexCharts from 'vue3-apexcharts';

export default {
  props: {
    sensorData: Array,
  },
  name: 'ChartComponent',
  components: {
    apexcharts: VueApexCharts,
  },
  data: function() {
    return {
      ready: false,
      series: [],
      chartOptions: {
        chart: {
          height: 350,
          type: 'line',
          dropShadow: {
            enabled: true,
            color: '#000',
            top: 18,
            left: 7,
            blur: 10,
            opacity: 0.2
          },
          toolbar: {
            show: false
          }
        },
        colors: ['#77B6EA', '#545454'],
        dataLabels: {
          enabled: false,
        },
        stroke: {
          curve: 'smooth'
        },
        title: {
          text: 'Températures par capteurs',
          align: 'left'
        },
        grid: {
          borderColor: '#e7e7e7',
          row: {
            colors: ['#f3f3f3', 'transparent'], // takes an array which will be repeated on columns
            opacity: 0.5
          },
        },
        markers: {
          size: 0,
        },
        xaxis: {
          categories: [],
          title: {
            text: 'Date'
          }
        },
        yaxis: {
          title: {
            text: 'Temperature'
          },
          min: 0,
          max: 0
        },
        legend: {
          position: 'top',
          horizontalAlign: 'right',
          floating: true,
          offsetY: -25,
          offsetX: -5
        }
      },
    }
  },
  watch: {
    sensorData(newData) {
      let processedSensorData = new Map();
      const dates = [];

      let minTemp = 0;
      let maxTemp = 0;

      for (let sensor of newData) {
        if (sensor.id === undefined) continue;
        processedSensorData.set(sensor.capteur, []);

        if (!dates.includes(sensor.received_at))
          dates.push((sensor.received_at));

        minTemp = Math.min(minTemp, sensor.temperature_relevee);
        maxTemp = Math.max(maxTemp, sensor.temperature_relevee);
      }

      // Pour chaque date,
      for (const date of dates) {
        // On prend chaque sensor...
        for (const [k, v] of processedSensorData.entries()) {
          // Et on lui attribue une température nulle pour cette date.
          let temp = null;

          for (const sensorHistoryPoint of newData) {
            if (sensorHistoryPoint.capteur !== k) continue;

            // Et s'il a une température pour cette date, alors on la prend.
            if (sensorHistoryPoint.received_at === date)
              temp = sensorHistoryPoint.temperature_relevee;
          }

          // Et on la stocke.
          v.push(temp);
          processedSensorData.set(k, v);
        }
      }

      let aled = Array.from(processedSensorData, ([name, data]) => ({ name, data }));
      console.log(aled);

      this.series = aled;
      this.chartOptions.xaxis.categories = dates;
      this.chartOptions.yaxis.min = minTemp - 5;
      this.chartOptions.yaxis.max = maxTemp + 5;
      this.chartOptions.markers.size = Array(aled.length).fill(0);

      console.log(minTemp, maxTemp);
      this.ready = true;
    }
  }
}
</script>

<style scoped>

</style>
