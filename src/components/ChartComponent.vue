<template>
  <div id="chart" v-for="([,item], index) in charts.entries()" v-bind:key="index">
    <apexcharts type="line" height="350" :options="item.chartOptions" :series="item.series"></apexcharts>
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
      charts: [],
    }
  },
  watch: {
    sensorData(newData) {
      let processedSensorDataTemp = new Map();
      let processedSensorDataHumid = new Map();
      const dates = [];

      let minTemp = 0;
      let maxTemp = 0;

      for (let sensor of newData) {
        if (sensor.id === undefined) continue;
        processedSensorDataTemp.set(sensor.capteur, []);
        processedSensorDataHumid.set(sensor.capteur, []);

        if (!dates.includes(sensor.received_at))
          dates.push((sensor.received_at));

        minTemp = Math.min(minTemp, sensor.temperature_relevee);
        maxTemp = Math.max(maxTemp, sensor.temperature_relevee);
      }



      // Pour chaque date,
      for (const date of dates) {
        // On prend chaque sensor...
        for (const [k, v] of processedSensorDataTemp.entries()) {
          // Et on lui attribue une température nulle pour cette date.
          let tempTemperature = null;

          for (const sensorHistoryPoint of newData) {
            if (sensorHistoryPoint.capteur !== k) continue;

            // Et s'il a une température pour cette date, alors on la prend.
            if (sensorHistoryPoint.received_at === date)
              tempTemperature = sensorHistoryPoint.temperature_relevee;
          }

          // Et on la stocke.
          v.push(tempTemperature);
          processedSensorDataTemp.set(k, v);
        }
      }



      // Pour chaque date,
      for (const date of dates) {
        // On prend chaque sensor...
        for (const [k, v] of processedSensorDataHumid.entries()) {
          // Et on lui attribue une température nulle pour cette date.
          let tempHumidite = null;

          for (const sensorHistoryPoint of newData) {
            if (sensorHistoryPoint.capteur !== k) continue;

            // Et s'il a une température pour cette date, alors on la prend.
            if (sensorHistoryPoint.received_at === date)
              tempHumidite = sensorHistoryPoint.niveau_humidite;
          }

          // Et on la stocke.
          v.push(tempHumidite);
          processedSensorDataHumid.set(k, v);
        }
      }



      // let aled = Array.from(processedSensorDataTemp, ([name, data]) => ({ name, data }));

      for (const [k, v] of processedSensorDataTemp.entries()) {
        const options = {
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
            text: 'Température du capteur ' + k,
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
            categories: dates,
            title: {
              text: 'Date'
            }
          },
          yaxis: {
            title: {
              text: 'Temperature'
            },
            min: minTemp - 5,
            max: 100
          },
          legend: {
            position: 'top',
            horizontalAlign: 'right',
            floating: true,
            offsetY: -25,
            offsetX: -5
          }
        };

        this.charts.push({
          chartOptions: options,
          series: [
            {
              name: "Température",
              data: v
            },
            {
              name: "Humidité",
              data: processedSensorDataHumid.get(k)
            }
          ]
        });
      }

      console.log(this.charts);

      this.ready = true;
    }
  }
}
</script>

<style scoped>

</style>
