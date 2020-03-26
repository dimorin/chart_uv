<template>
  <div id="app">
    <h1>UV</h1>    
    <section class="from">
      <div>chart library : chart.js</div>
      <div>data api from <a href="https://openweathermap.org/" target="_blank">https://openweathermap.org/</a></div>
      <div>Ultraviolet index from <a href="https://en.wikipedia.org/wiki/Ultraviolet_index" target="_blank">https://en.wikipedia.org/wiki/Ultraviolet_index</a></div>
      <div>다음 스텝 : vuex, css framework</div>
    </section>    
    <div style="font-size:0;">
      <div style="display:inline-block;width:50%;">
        <bar-chart :dataset ="chartDataSet" :labels="chartLabels" @update_city="update_city"></bar-chart>
      </div>
      <div style="display:inline-block;width:50%;">
        <line-chart :dataset="chartDataSet" :labels="chartLabels" @update_city="update_city"></line-chart>
      </div>      
    </div>  
    <bar-chart-d3></bar-chart-d3>      
  </div>
</template>

<script>
import axios from 'axios'
import BarChart from '@/components/BarChart.vue'
import LineChart from '@/components/LineChart.vue'
import BarChartD3 from '@/components/BarChartD3.vue'

export default {
  name: 'App',
  data(){
    return{
      chartLabels:[],
      chartDataSet :[]
    }
  },  
  components: {
    BarChart,
    LineChart,
    BarChartD3,
  },
  methods:{
    update_city(city){
      var lat = null;
      var lon = null;
      
      switch(city){
        case 'seoul': lat=37.5683; lon=126.9778; break;
        case 'busan': lat=35.1028; lon=129.0403; break;
        case 'jeju': lat=33.5097; lon=126.52198; break;
        default: lat=37.5683; lon=126.9778;
      }
      axios.get(`https://api.openweathermap.org/data/2.5/uvi/forecast?lat=${lat}&lon=${lon}&appid=${process.env.VUE_APP_ID}`)
      .then(response => {      
        this.chartLabels = response.data.map(el => el.date_iso);
        this.chartDataSet = response.data.map(el => el.value);
      })
    },
  },
  created(){    
    this.update_city('seoul');    
  }, 
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
section.from{margin:10px;border:1px solid #dddddd;padding:10px;text-align:left;}
</style>
