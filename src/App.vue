<template>
  <div id="app">
    <div class="navbar">
      <div class="city">
      <h2 v-if="!searchResult">la Météo de</h2>
      <h1 v-if="!searchResult">{{weather.cityName}}</h1>
      <p v-if="!searchResult"> {{weather.country}}</p>
      <div class="not-found" v-if="searchResult"> Ville non trouver</div>
      <form class='search-location' action="" @submit.prevent="getWeather">
      <input type="text" placeholder="Choisis la ville" v-model="citySearch" autocomplete='off'>
      </form>
      </div>
      <img src="./assets/logo.png" alt="Logo">
    </div>
    <section class="main_meteo">
      <div>
        <p>Temperature actuelle</p>
      <h2>{{weather.temperature}}°C</h2>
    <section class="details">
      <p>Min {{weather.lowTemp}}°C</p>
      <p>Max {{weather.highTemp}}°C</p>
      <p>Ressentie{{weather.feelsLike}}°C</p>
    </section>
      </div>
      <div>
      <p> {{weather.description}}</p>
      <i class="weatherDescription"></i>
      <!-- <i class="fas fa-location-arrow arrow-wind"></i> -->
      <p> {{weather.wind}}Km/h</p>
      <p>{{weather.windDirection}}deg</p>
      </div>
      <div>
      <i class="dayNight fa-4x fas " :class="isDay ? 'fa-sun' : 'fa-moon' "> </i>
        <p>Humidité</p>
      <p>{{weather.humidity}}%</p>




      </div>
    </section>



  </div>
</template>

<script>

export default {
  name: 'App',
  components: {},
data () {
return {
  isDay:true,
  searchResult:false,
  citySearch: "",
weather : {
cityName : "Bordeaux",
country : "FR",
temperature : 9,
description : "Nuageux",
lowTemp: 0,
highTemp : 15,
feelsLike: 7,
humidity : 55,
wind : 5,
windDirection: 180,
},
}
},
methods: {
  getWeather: async function(){
    console.log(this.citySearch);
    const key = "64c46098d3af0a099a1f232581608d1e";
    const callURL = `https://api.openweathermap.org/data/2.5/weather?q=${this.citySearch}&appid=${key}&units=metric&lang=fr`;
    // appel de l'API avec un await
    try {
      const response = await fetch(callURL)
      const data = await response.json();
      console.log(data)
      this.citySearch="";
      this.weather.cityName=data.name;
      this.weather.country=data.sys.country;
      this.weather.temperature=Math.round(data.main.temp);
      this.weather.description=data.weather[0].description;
      this.weather.lowTemp=Math.round(data.main.temp_min);
      this.weather.highTemp=Math.round(data.main.temp_max);
      this.weather.feelsLike=Math.round(data.main.feels_like);
      this.weather.humidity=Math.round(data.main.humidity);
      this.weather.windDirection=Math.round(data.wind.deg);

      
      this.searchResult=false;
      // check day or night
    const dayNight = data.weather[0].icon;
    if(dayNight.includes('d')){
      this.isDay= true
    }
    else{
      this.isDay= false
    }
    const description = this.weather.description;
    const weatherDescription = document.querySelector('.weatherDescription')

    if(description=='ciel dégagé'){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain"),
      weatherDescription.classList.add("fas", "fa-4x" ,"weatherDescription", "fa-sun");
    }
    if(description.includes('nuageux')){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain"),
      weatherDescription.classList.add("fas", "fa-4x" ,"weatherDescription" , "fa-cloud-sun");
      
    }
    if(description=='couvert'){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain"),
      weatherDescription.classList.add("fas", "fa-4x" ,"weatherDescription" , "fa-cloud");

    }
    if(description.includes('pluie')){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain"),
      weatherDescription.classList.add("fas", "fa-4x" ,"weatherDescription" , "fa-cloud-rain");
      
      }

      
    } catch (error) {
      console.log(error)
      this.searchResult=true;
    }

  }
},
}
</script>

<style>
*{
  font-family: Helvetica, Arial, sans-serif;
  text-align: center;
  margin:0;
  padding:0
}
/* navbar */
.navbar{
  background-color:rgb(12, 15, 168);
  width:100vw;
  height:200px;
  display:flex;
  flex-direction:row;
  justify-content: space-around;
  align-items: center
}
.city h2{
  color: white;
  text-align: left ;
  font-size:0.8rem
}
.city p{
  color: white;
}
h1{
  font-size:4rem;
  color: white;
}
.navbar img{
  height:150px;
  width:150px
}
.city input{
  width:300px;
  transform: translateY(100%);
  color: gray;
}
/* main */
.fa-sun{
  color: yellow;

}
.main_meteo{
  display:flex;
  justify-content: center;
  align-items: center;
  margin:40px auto;
  width: 60vw;
  background-color:rgb(218, 218, 218);
  height:300px;
  border-radius:50px;
}
.main_meteo div{
  width:30vw;
}
.main_meteo div:nth-child(2){
  height:80%;
  border-left: 1px black solid;
  border-right: 1px black solid;
  display:flex;
  flex-direction:column;
  justify-content: center;
  align-items: center;
}
.details{
    margin-top: 75px;
  margin-left: 50px;
}
.details p{
  text-align:left;

  
}





</style>
