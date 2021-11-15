<template>
  <div class="WeatherInfoCard">
    <div  v-if="!err">
 <div class="main">
    <h2>
      {{data.name}}
    </h2>
    <h1>
      {{data.main.temp.toFixed()}}°С
    </h1>
    <h2>
      {{data.weather[0].description}}
    </h2>
    </div>
    
    <div class="info">
    <p>
      Feels like {{data.main.feels_like}}°С
    </p>
    <p>
      Wind {{data.wind.speed}} m/s
    </p>
    <p>
      Visibility {{data.visibility/1000}} km
    </p>
    <p>
      Pressure  {{data.main.pressure*3/4}} mm
    </p>
    <p>
      Humidity {{data.main.humidity}}%
    </p>
    <p>
      Dew Point {{dewPoint}}°С
    </p>
    </div>
    </div>
    <div  v-else>
      <p>Полилка</p>
    </div>
    
  </div>
</template>

<script>

import Vue from "vue";
import axios from "axios";
import VueAxios from "vue-axios";
Vue.use(VueAxios, axios)
export default {
  name: 'WeatherInfoCard',
  props: {
    latitude:{
      type: Number,
      default: null
    },
    longitude:{
      type: Number,
      default: null
    },
    city: {
      type: String,
      default: ''
    },
  },
  data: function(){
    return{
      data: {},
      err: false
    }
    
  },
  mounted:  function(){
    console.log('efewfewfewf' ,this.longitude, this.latitude, this.city)
    console.log(this.request)
    
    Vue.axios.get(`http://${this.link}`).then(response=>this.data = response.data).then(()=>console.log(this.data)).catch(()=> this.err = true)
    
  },
  computed: {
    dewPoint: function(){
      const a = 17.27
      const b = 237.7
      const T = this.data.main.temp
      const RH = this.data.main.humidity/100;
      
      const M = (   ((a*T)/(b+T))  + Math.log(RH))
      console.log(M)
      return ((b*M)/(a-M)).toFixed()
    },
    link: function(){
      const KEY = 'f326b5463f56551f2590394732cd10d7'
      if(this.city){
        console.log(`api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${KEY}&units=metric`)
        return `api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${KEY}&units=metric`
      } else if(this.latitude && this.longitude){
        return `api.openweathermap.org/data/2.5/weather?lat=${this.latitude}&lon=${this.longitude}&appid=${KEY}&units=metric`
      } else{
        return null
      }
    }
  },
  
}
</script>

<style scoped>
*{
  font-family: 'Roboto', sans-serif;
}
.WeatherInfoCard{
  width: 600px;
   display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  border: 2px solid cyan;
  border-radius: 20px;
}
.info{
  display: flex;
  flex-wrap: wrap;
}
.info p{
  margin: 5px 20px;
  display: block;
  width: 150px;
}
h2{
  font-size: 36px;
  font-weight: 400;
  margin: 0;
}
h1{
  font-size: 54px;
  font-weight: 400;
  margin: 0;
}
.main{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
