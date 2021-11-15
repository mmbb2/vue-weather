<template>
  <div class="WeatherInfoCard">
  
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
    request: {
      type: String,
      default: ''
    }
  },
  mounted: function(){
    console.log(this.request)
   Vue.axios.get(this.request).then(([data])=>console.log(data))
  }
}
</script>

<style scoped>

</style>
