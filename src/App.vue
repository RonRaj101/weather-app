<template>
  
  <h1>Weather App</h1>
  <div class="weather">
    <div class="search">
      <input v-model="query" @keypress.enter="getWeather" placeholder="Enter City Name" name="" id="searchbox">
      
    </div>

    <div :class="parseInt(main.temp) > 20 ? 'hot' : 'cold'" class="result">
      <div v-if="Object.keys(wData).length > 0">
        <h1>{{ wData.name }}, {{ sys.country }}</h1>
        <h1 class="temp">{{ Math.round(parseInt(main.temp)).toString() }}&#8451;</h1>
        <h2 style="text-transform:capitalize ;">{{ weather.description }}</h2>
      </div>
      <div v-else>
        <h1>Search for a country/city/territory & ENTER</h1>
      </div>
    </div>
    


  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      api_key:'82fdbb08ed00b8ce22770863b7dbc182',
      query:'karachi',
      wData:{},
      coords:{},
      main:{},
      sys:{},
      weather:{}
    }
  },
  methods:{
     getWeather(){
      fetch(`http://api.openweathermap.org/data/2.5/find?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then(response => {
          return response.json()
        }).then(this.setResults);  
    },
    setResults(result){
      this.wData = result.list[0];
      this.coords = this.wData.coord;
      this.main = this.wData.main;
      this.sys = this.wData.sys;
      this.weather = this.wData.weather[0];
    }
  }
}
</script>

<style>
#app {
  font-family: monospace, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

*{
  padding:0;
  margin:0;
  box-sizing: border-box;
}

#searchbox{
  font-size: 2rem;
  text-align: center;
}

.weather{
  margin: 1rem;
  padding:1rem;
  
}

.result{
  margin:2rem;
  padding:1rem;
  text-align: -webkit-center;
  line-height: 3;
}

.result > *{
  padding:1rem;
}

.hot{
  background: linear-gradient(45deg,crimson,white);
}

.cold{
  background: linear-gradient(135deg, aqua,white);
}

.temp{
    font-size: 4rem;
    width: fit-content;
    padding: 1rem;
}
</style>
