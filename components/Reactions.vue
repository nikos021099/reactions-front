<template>
  <v-flex xs12 class="reactions" v-if="reactions != []">
    <v-layout row>
      <v-flex xs12>
        <v-card>
          <v-card-text>
            {{ title }}
          </v-card-text>
          <v-layout row>
            <v-flex xs12>
              <v-layout>
                <v-flex xs2 v-for="react in reactions">
                  <ReactionCount :count="react.count" :reaction="react.reaction"/>
                </v-flex>
              </v-layout>
            </v-flex>
            <v-flex xs12 s12 m9 l9 xl9 class="chart">
                <vue-apex-charts type="bar" height="300"  :options="barChartOptions.options" :series="barChartOptions.series"/>
            </v-flex>
            <v-flex xs12 s12 m3 l3 xl3 class="chart">
              <vue-apex-charts type="pie" height="300"  :options="donutChartOptions.options" :series="donutChartOptions.series"/>
            </v-flex>
          </v-layout>
        </v-card>
      </v-flex>
    </v-layout>
  </v-flex>
</template>

<script>
    import ReactionCount from "./ReactionCount";
    import VueApexCharts from 'vue-apexcharts'

    export default {
      name: "People",
      props:['reactions', 'title'],
      computed:{
        barChartOptions: function () {
          return {
            options: {
              chart: {
                id: "barChart"
              },
              xaxis: {
                categories: this.reactions ? this.reactions.map(x => x.reaction) : [],
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
              data: this.reactions ? this.reactions.map(x => x.count) : []
            }],

          }

        },
        donutChartOptions: function () {
          return {
            options: {
              labels: this.reactions ? this.reactions.map(x => x.reaction) : [],
              legend: {
                labels: {
                  colors: "white"
                }
              }
            },
            series: this.reactions ? this.reactions.map(x => x.count) : [],

          }
        },
      },


      components: {ReactionCount, VueApexCharts}
    }
</script>

<style scoped>
  .reactions{
      padding: 10px !important;
  }


</style>
