<template>
  <div id="app">
    <WeatherInfoCard :request="`api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&appid=f326b5463f56551f2590394732cd10d7`"/>
  </div>
</template>

<script>
import WeatherInfoCard from './components/WeatherInfoCard.vue'

import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios)
export default {
  name: 'App',
  data: function(){
    return{
      latitude: null,
      longitude: null
    }
  },
  components: {
    WeatherInfoCard
  },
  mounted: function(){
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(({coords})=>{
        this.latitude = coords.latitude
        this.longitude = coords.longitude
        console.log( this.latitude,  this.longitude)
      })
    
  } else {
    console.log('wefef')
  }
  }
}
</script>

<style>

</style>
