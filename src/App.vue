<template>
  <div id="app" :class="typeof weather.location != 'undefined' && weather.current.temp_c > 16 ? 'warm':''">
    <div class="error" v-show="weather=='undefined'">
      <h1>Please enter the correct country :(</h1>
    </div>
    <main>
      <div class="search-box" >
        <input 
          type="text" 
          placeholder="search..." 
          class="search-bar"
          v-model="query"
          @keypress="fetchweather"
        >
      <div class="weather-wrap" v-if="weather.location">
        <div class="location-box">
          <div class="location" >{{weather.location.country}}</div>
          <div class="date">{{getDate()}}</div>
        </div>
      </div>
      <div class="weather-box" v-if="weather.location">
        <div class="temp">{{weather.current.temp_c}}°c</div>
          <div class="weather">
            <img :src="weather.current.condition.icon"/>
            {{weather.current.condition.text}}
          </div>
      </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: "642ad4e024d3475e8f363556221109",
      url_base:`https://api.weatherapi.com/v1/current.json?`,
      query: '',
      weather: {}
    };
  },
  methods: {
        fetchweather (e) {
          if( e.key == "Enter"){
            fetch(`https://api.weatherapi.com/v1/current.json?key=${this.api_key}&q=${this.query}&aqi=yes`).then(res => res.json()).then(this.setResults);
        }
      },
      setResults(results) {
        if(results==undefined){
          this.weather=undefined;
        }
        else{
          this.weather=results;
        }
      },
      getDate(){
        let d = new Date();
        let days =["Sunday" , "Monday" , "Tuesday", "Wenesday", "Tharusday","Friday","Starday"];
        let months = ["January" , "February" , "March" , "April" , "May" ,"june", "July" , "August" , "September" , "October" , "November" , "December"];
        let day =days[d.getDay()];
        let month = months[d.getMonth()];
        let date = d.getDate();
        let year = d.getFullYear();
        return `${day} ${date} ${month} ${year}`
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
  body {
    font-family: "montserrat" , sans-serif;
  }
  #app {
    background-image: url("./assets/img/cold_bg.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: bottom;
    transition: 0.4s;
  }
  #app.warm{
    background-image: url("./assets/img/warm_bg.jpg");
  }
  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom , rgba(0 ,0,0 , 0.25) , rgb(0,0,0,0.75));
  }
  .search-box{
    width: 100%;
    margin-bottom: 30px;
  }
  .search-box .search-bar {
    display: block;
    width: 100%;
    padding: 15px;
    color: #313131;
    font-size: 20px;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 225, 0.5);
    border-radius: 0px 16px 0px 16px;
    transition: 0.4s;
    box-shadow: 0 0 8px rgba(0, 0, 0, 0.25);
  }
  .search-box .search-bar:focus {
    background-color: rgba(255, 255, 255 , 0.75);
    box-shadow: 0 0 16px rgba(0, 0, 0, 0.75);
    border-radius: 16px 0px 16px 0px;
  }
  .weather-wrap{
    margin: 20px 0 ;
  }
  .location-box .location{
    color: #FFF;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
  }
  .location-box .date {
    color: #FFF;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
    margin: 10px 0;
  }
  .weather-box{
    text-align: center;
  }
  .weather-box .temp{
    display: inline-block;
    padding: 10px 25px;
    color: #FFF;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }
  .weather-box .weather {
    color: #FFF;
    font-size: 48px;
    font-weight: 700;
    text-shadow: 3px 6px rgba(0,0,0,0.25);
  }
</style>

