<template>
  <div id="app">
    <div class="navbar">
      <div class="city">
      <h2 v-if="!searchResult">la Météo de</h2>
      <div class="city-daynight">
      <h1 v-if="!searchResult">{{weather.cityName}}</h1>
      <i class="dayNight fa-4x fas " :class="isDay ? 'fa-sun' : 'fa-moon' "> </i>
      </div>
      <p v-if="!searchResult"> {{weather.country}}</p>
      <div class="not-found" v-if="searchResult"> Ville non trouvée</div>
      </div>
      <img src="./assets/logo.png" alt="Logo">
    </div>
      <form class='search-location' action="" @submit.prevent="getWeather">
      <input type="text" placeholder="Choisis la ville" v-model="citySearch" autocomplete='off'>
      </form>
    <section class="main_meteo">
      <div>
        <p>Temperature actuelle</p>
      <h2>{{weather.temperature}}°C</h2>
      </div>
      <div>
      <h3> {{weather.description}}</h3>
      <i class="weatherDescription"></i>
      </div>
      <div>
        <p>Humidité</p>
      <h2>{{weather.humidity}}%</h2>
      </div>
    </section>
      
    <section class="other">
    <div class="detailed_temps" >
      <p>Min {{weather.lowTemp}}°C</p>
      <p>Max {{weather.highTemp}}°C</p>
      <p>Ressentie{{weather.feelsLike}}°C</p>
    </div>
    <div class="wind">
      <i class="fas fa-location-arrow fleche fa-2x"></i>

      <p> {{weather.wind}}Km/h</p>
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
cityName : "?",
country : "?",
temperature : "?",
description : "?",
lowTemp: "?",
highTemp : "?",
feelsLike: "?",
humidity : "?",
wind : "?",
windDirection: "?",
},
}
},
methods: {
  getWeather: async function(){
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
      this.weather.wind=Math.round(Math.round(data.wind.speed*1.852));
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
    const fleche= document.querySelector('.fleche')
    const windir = this.weather.windDirection
    const wind2 =windir-135
    console.log(wind2)
    fleche.style.transform=`rotateZ(${wind2}deg`






    const description = this.weather.description;
    const weatherDescription = document.querySelector('.weatherDescription')

    if(description=='ciel dégagé'){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain", "fa-snowflake","fa-smog"),
      weatherDescription.classList.add("fas", "fa-4x" , "fa-sun");
    }
    if(description.includes('nuageux')){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain", "fa-snowflake","fa-smog"),
      weatherDescription.classList.add("fas", "fa-4x" , "fa-cloud-sun");
      
    }
    if(description=='couvert'){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain", "fa-snowflake","fa-smog"),
      weatherDescription.classList.add("fas", "fa-4x" , "fa-cloud");

    }
    if(description.includes('pluie')){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain", "fa-snowflake","fa-smog"),
      weatherDescription.classList.add("fas", "fa-4x"  , "fa-cloud-rain");
    }
    if(description.includes('brume')){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain", "fa-snowflake","fa-smog"),
      weatherDescription.classList.add("fas", "fa-4x"  , "fa-smog");
     }
    if(description.includes('neige')){
      weatherDescription.classList.remove("fa-sun","fa-cloud-sun","fa-cloud","fa-cloud-rain", "fa-snowflake","fa-smog"),
      weatherDescription.classList.add("fas", "fa-4x"  , "fa-snowflake");
     }    
    } catch (error) {
      console.log(error)
      this.searchResult=true;
      this.weather.temperature="?";
      this.weather.description="?";
      this.weather.lowTemp="?";
      this.weather.highTemp="?";
      this.weather.feelsLike="?";
      this.weather.humidity="?";
      this.weather.windDirection="?";
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
.city-daynight{
  display: flex;
}
h1{
  font-size:4rem;
  color: white;
}
.navbar img{
  height:150px;
  width:150px
}
 input{
  width:600px;
  height: 50px;
  color: black;
  font-size: 3rem;
  margin-top: 30px;
}
/* main */
.fa-sun{
  color: yellow;
}
.fa-moon{
  color: ghostwhite;
}
.fa-cloud, .fa-cloud-rain, .fa-cloud-sun{
  color: gray;
}
.fa-smog{
  color: gray;
}
.fa-snowflake{
  color: ghostwhite;
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
.main_meteo h2{
  font-size: 3rem;
}
.main_meteo h3{
  font-size: 2rem;
  margin-bottom:20px ;
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

.details p{
  text-align:left;
}
.not-found{
  color: red;
  font-size: 3rem;
}
.other{
  display: flex;
  justify-content: center;
}
.detailed_temps{
  width: 15%;
  height: 100px;
  background-color: #DADADA;
  margin-right: 200px;
  border-radius: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.wind{
  width: 15%;
  height: 100px;
  background-color: #DADADA;
  border-radius: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}






</style>
