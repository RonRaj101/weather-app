<template>
  
  <h1>Weather App</h1>
  <div class="weather">
    <div class="search">
      <input v-model="query" @keypress.enter="getWeather" placeholder="Enter City Name" name="" id="searchbox" required>
    </div>

    
    <div v-if="objState && typeof wData != 'undefined'">
      <div class="result" :class=" parseInt(wData.main.temp) > 20 ? 'hot' : 'cold' " >
        <h1>{{ wData.name }}, {{ wData.sys.country }}</h1>
        <h1 class="temp">{{ Math.round(parseInt(wData.main.temp)).toString() }}&#8451;</h1>
        <h2 style="text-transform:capitalize ;">{{ wData.weather[0].description }}</h2>
      </div>
    </div>
    <div class="result" v-else>
        <p>{{ elseText }}</p>
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
      elseText: 'Search for a country/city/territory & ENTER',
      wData:{},
      objState: false
    }
  },
  methods:{
     getWeather(){
     
      fetch(`http://api.openweathermap.org/data/2.5/find?q=${this.query}&units=metric&APPID=${this.api_key}`)
      .then(async response => {
          const result = await response.json();

          if(!response.ok){
            const error = (result && result.message) || response.statusText;
            return Promise.reject(error);
          }

          this.objState = true;
          this.wData = result.list[0];
      })
      .catch(error => {
        this.elseText = "Enter a valid location name!";
        this.objState = false;
        console.log(error);
      })
      
      
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Fira+Sans:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;1,100;1,200;1,300;1,400;1,500;1,600;1,700&display=swap');

#app {
  font-family: 'Fira Sans', sans-serif;
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
  max-width: 70vw;
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
  max-width: 90vw;
  transition-property: background;
  transition-duration: 2s;
  transition-timing-function: linear;
  transition-delay: 1s;
}

.result > *{
  padding:1rem;
}

.result.hot{
  background: linear-gradient(45deg,crimson,white);
}

.result.cold{
  background: linear-gradient(135deg, aqua,white);
}

.temp{
    font-size: 4rem;
    width: fit-content;
    padding: 1rem;
    box-shadow: 5px 5px 10px 0px;
}
</style>
