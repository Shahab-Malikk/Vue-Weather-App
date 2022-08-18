<template>

  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp>17?'warm':''">
<main>
  <div class="search-box">
    <input type="text" class="search-bar" placeholder="Enter City..." v-model="query" @keypress="fetchWeather">
  </div>
  <div class="weather-wrap" v-if= "typeof weather.main != 'undefined'">
    <div class="location-box">
      <div class="location">
     {{weather.name}}, {{weather.sys.country}}
      </div>
      <div class="date">
        <!-- Thursday 17 January 2022 -->
        {{dateBuilder()}}
      </div>
    </div>
  </div>
  <div class="weather-box"  v-if= "typeof weather.main != 'undefined'"> 
    <div class="temp">
{{ Math.round(weather.main.temp)}}°C
    </div>
    <div class="weather">
   {{weather.weather[0].main}}
    </div>
  </div>
  <div class="more-info"  v-if= "typeof weather.main != 'undefined'">
    <h2>More Info</h2> 
  <div class="info-container">
<div class="info-box">
    <div class="sun_rise">
    <i class="fa-solid fa-sun"></i> Sunrise : {{ getTime(Math.round(weather.sys.sunrise))}}
  </div>
   <div class="min_temp">
<i class="fa-solid fa-sun"></i>  Sunset : {{ getTime(Math.round(weather.sys.sunset))}}
  </div>
  <div class="min_temp">
     <i class="fa-solid fa-temperature-low"></i>Min Temparature : {{ Math.round(weather.main.temp_min)}}°C
  </div>
   <div class="max_temp">
    <i class="fa-solid fa-temperature-high"></i>Max Temparature : {{ Math.round(weather.main.temp_max)}}°C
  </div>
    <div class="wind">
<i class="fa-solid fa-wind"></i>  Wind : {{ weather.wind.speed}} m/s
  </div>
 
 
 <!-- <div class="temp_container">
<i class="fa-solid fa-sun"></i>
<i class="fa-solid fa-face-smile"></i>

<i class="fa-solid fa-sunset"></i>
<i class="fa-solid fa-droplet-degree"></i>




                                   
   {{weather.main.feels_like}}
  
 </div> -->
</div>



  </div>
  </div>
</main>
   </div>
</template>

<script>
export default {
 name:'app',
 data(){
return{
  apiKey:'76631b6adb19d22b193aef2e218d3662',
  urlBase:'https://api.openweathermap.org/data/2.5/',
  query:'',
  weather:{}

}
 },
 methods:{
fetchWeather(e){
if(e.key=="Enter"){
  fetch(`${this.urlBase}weather?q=${this.query}&units=metric&APPID=${this.apiKey}`)
  .then(res=>{
    return res.json();
  }).then(this.setResults);
}
},
setResults(results){
  this.weather=results;
},
dateBuilder(){
 let date=new Date();
let months=["January","February","March","April","May","June","July","August","September","October","November","December"]
let days=["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"]


let day=days[date.getDay()]
let datee=date.getDate()
let month=months[date.getMonth()]
let year= date.getFullYear()
 return  `${day} ${datee} ${month} , ${year}`;
},
getTime(timeStamp){
  let inMilis=timeStamp*1000;
  const dateObject=new Date(inMilis);
  let hour=  dateObject.getHours();
  let minutes=  dateObject.getMinutes();
  let str= hour>=12?' PM':' AM'
  hour=  dateObject.getHours()<12?`${dateObject.getHours()}`:`${dateObject.getHours()-12}`;
  return `${hour} : ${minutes} ${str} `
 

//   dateObject.toLocaleString("en-US", {hour: "numeric"}) // 10 AM
// dateObject.toLocaleString("en-US", {minute: "numeric"}) // 30
// dateObject.toLocaleString("en-US", {second: "numeric"}) // 15

}
 }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'montserrat',sans-serif;
}
#app{
  background: url('./assets/weatherCold.jpg');
  background-size: cover;
  background-position: bottom;
  /* transition: .5s; */
}
#app.warm{
   background: url('./assets/weatherHot.jpg');
   background-size: cover;
  background-position: bottom;
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25),rgba(0,0,0,.75));
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding:15px;
  font-size: 20px;
  color: #313131;
  appearance: none;
  border: none;
  background: none;
  outline: none;
  background-color: rgba(255,255,255,0.5);
  border-radius: 0 16px 0 16px;
  transition: 0.4s;
  box-shadow: 0px 0px 8px rgba(0,0,0,.25);
}
.search-box .search-bar:focus{
   box-shadow: 0px 0px 16px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,0.75);
border-radius: 16px 0px 16px 0px;
}
.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0,0,0,.25);
}
.location-box .date{
    color: #fff;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
 font-style: italic;
}
.weather-box{
  text-align: center;
}
.weather-box .temp{
  display: inline-block;
  font-size: 102px;
  padding: 10px 25px;
  color: #fff;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0,0,0,.25);
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,.25) ;
}
.weather-box .weather{
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;

  box-shadow: 3px 6px rgba(0,0,0,0.25);
  margin-bottom: 40px;
}
.fa-solid{
  color: yellow;
  font-size: 20px;
  margin-right: 10px;
}
.info-container{
  display: flex;
  justify-content: center;
  align-content: center;
}
.info-box{
 
  font-size: 16px;
  padding: 25px 15%;
  color: #fff;
  font-weight: 600;
  
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0,0,0,.25) ;
 



}
h2{
  text-align: center;
  color: #fff;
}
.info-box div{
  margin-bottom: 15px;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
