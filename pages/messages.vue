<style>
  .input-field {
    margin-right: 20px !important;
    margin-left: 20px !important;
    margin-top: 20px !important;
  }

  .chart-card {
    margin-top: 10px;
    margin-right:5px ;
  }

  .donut-card {
    margin-top: 10px;
  }
</style>

<template>
  <v-layout
    row
  >
    <v-flex xs12 s12 m12 l12 xl12>
      <v-card class="input-card">
        <v-text-field
          label="Query"
          full-width
          class="input-field"
          @change="update"
          v-model="query"
        />
        <!--      <CountStanding position="Position" name="Name"-->
        <!--                     count="Count"/>-->
        <!--      <CountStanding v-for="(standing, index) in standings" :position="index + 1" :name="standing.sender"-->
        <!--                     :count="standing.count"/>-->
      </v-card>
    </v-flex>

    <v-flex xs8>
      <v-card class="chart-card" v-if="query">
        <v-card-text>Search Results for: '{{ queried }}'</v-card-text>
        <div>
          <vue-apex-charts type="bar" height="600" :options="barChartOptions.options" :series="barChartOptions.series"/>
        </div>
      </v-card>
    </v-flex>
    <v-flex xs4>
      <v-card class="donut-card" v-if="query">
        <v-card-text>Search Results for: '{{ queried }}'</v-card-text>
        <div>
          <vue-apex-charts height="600" type="pie" :options="donutChartOptions.options"
                           :series="donutChartOptions.series"/>
        </div>
      </v-card>
    </v-flex>

  </v-layout>
</template>


<script>
  import CountStanding from "../components/CountStanding";
  import axios from "~/plugins/axios";
  import VueApexCharts from 'vue-apexcharts'

  export default {
    components: {CountStanding, VueApexCharts},
    data: function () {
      return {
        query: "",
        queried: "",
        standings: []
      }
    },
    computed: {
      barChartOptions: function () {
        return {
          options: {
            chart: {
              id: "barChart"
            },
            xaxis: {
              categories: this.standings ? this.standings.map(x => x.sender) : [],
              labels: {
                style: {
                  colors: '#FFF'
                },
                trim: false,
                maxHeight: 1000
              }
            },
            yaxis: {
              labels: {
                style: {
                  color: '#FFF'
                },
              }
            },
            dataLabels: {
              style: {
                colors: ['white']
              }
            },
            markers: {
              style: {
                colors: ['black']
              }
            },
            fill: {
              colors: ['#1A73E8', '#B32824']
            },
            colors: ['#F47B20', '#005596'],
          },
          series: [{
            name: 'Message Count',
            data: this.standings ? this.standings.map(x => x.count) : []
          }],

        }

      },
      donutChartOptions: function () {
        return {
          options: {
            labels: this.standings ? this.standings.map(x => x.sender) : [],
            legend: {
              labels: {
                colors: "white"
              }
            }
          },
          series: this.standings ? this.standings.map(x => x.count) : [],

        }
      }
    },
    methods: {
      'update': async function () {
        let response = await axios.get("/phrases", {params: {phrase: this.query}});
        this.queried = this.query;
        this.standings = response.data;
      }

    }


  }
</script>
