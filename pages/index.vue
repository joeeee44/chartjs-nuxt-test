<template>
  <v-container class="small">
    <line-chart
      :styles="myStyles"
      :chart-data="datacollection"
      :options="options"
    ></line-chart>
    <v-btn dark color="indigo" @click="fillData()">Randomize</v-btn>
    <v-btn @click="increase()">Increase height</v-btn>
  </v-container>
</template>

<script>
import LineChart from '@/plugins/LineChart.js'

export default {
  components: {
    LineChart
  },
  data() {
    return {
      datacollection: {},
      options: {
        responsive: true,
        maintainAspectRatio: false
      },
      height: 300
    }
  },
  computed: {
    myStyles() {
      return {
        height: `${this.height}px`,
        position: 'relative'
      }
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    increase() {
      this.height += 10
    },
    fillData() {
      this.datacollection = {
        labels: [...Array(31)].map((item, i) => {
          return i + 1
        }),
        datasets: [
          {
            lineTension: 0,
            label: 'Data One',
            backgroundColor: 'rgba(0, 0, 0, 0)',
            borderColor: 'rgba(117, 202, 251, 1)',
            data: [...Array(31)].map((item, i) => {
              return this.getRandomInt()
            })
          },
          {
            lineTension: 0,
            label: 'Data One',
            backgroundColor: 'rgba(0, 0, 0, 0)',
            borderColor: 'rgba(252, 121, 118, 1)',
            data: [...Array(31)].map((item, i) => {
              return this.getRandomInt()
            })
          }
        ]
      }
    },
    getRandomInt() {
      return Math.floor(Math.random() * (50 - 5 + 1)) + 5
    }
  }
}
</script>

<style>
/* .chartjs-size-monitor { */
/*   position: relative; */
/*   margin: auto; */
/*   height: 50vh; */
/*   width: 80vw; */
/* } */
</style>
