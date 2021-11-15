<template>
  <div id="app">
    <WeatherInfoCard v-if="(latitude && longitude)" :latitude="latitude" :longitude="longitude" :city="null"/>
    <input class="inputCity" v-model="inputCity" type="text">
    <button class="button" v-on:click="addCity">додати місто</button>
    <select class="select" v-model="city" name="" id="">
      <option class="option" v-for="(city, index) in cities" :key="index" :value="city"> {{city}} </option>
    </select>
    <WeatherInfoCard v-if="city" :key="city" :latitude="latitude" :longitude="longitude" :city="city"/>
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
      longitude: null,
      inputCity: '',
      cities: [],
      city: ''
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
      })
    
  } else {
    console.log('wefef')
  }
   this.cities =  JSON.parse(localStorage.getItem('cities')) ? JSON.parse(localStorage.getItem('cities')) : []
  },
  methods:{
    addCity: function(){
      if(this.inputCity){
      let cities =  JSON.parse(localStorage.getItem('cities')) 
       if(!cities){
         cities = [this.inputCity]
       } else{
      cities.push(this.inputCity)
       }
      localStorage.setItem ('cities', JSON.stringify(cities))
      this.cities.push(this.inputCity)
      } else{
        alert('Введіть місто')
      }
       
    },

  }
}
</script>

<style scoped>
*{
  font-family: 'Roboto', sans-serif;
}

.inputCity, .button, .select, .option{
  margin: 10px;
  font-size: 20px;
   border: 2px solid cyan;
  border-radius: 20px;
  background: white;
}
.select{
  width: 200px;
}
</style>
