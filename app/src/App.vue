<template>
  <div id="app" class="app" :class="[
      {'rainy': typeof codex.weather?.[0].main != 'undefined' && codex.weather?.[0].main === 'Rain'},
      {'snowy': typeof codex.weather?.[0].main != 'undefined' && codex.weather?.[0].main === 'Snow'},
      {'clear': typeof codex.weather?.[0].main != 'undefined' && codex.weather?.[0].main === 'Clear'},
      {'cloudy': typeof codex.weather?.[0].main != 'undefined' && codex.weather?.[0].main === 'Clouds'}]">
    <input v-model="query" type="text" class="searchbar" placeholder="Enter city name..." @keypress="getWeather">
      <div id="location" :class="typeof codex.name === 'undefined' ? 'hidden' : ''">
       {{ codex.name }}, {{ codex.sys?.country }}
      </div>
      <div id="date">{{getDate()}}</div>
      <div id="temp" :class="isNaN(codex.main?.temp) ? 'hidden' : ''">{{Math.round(codex.main?.temp)}}°C</div>
      <div id="description">{{ codex.weather?.[0].description }}</div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      key: "ddd37312d21544e7300b23a69a2fff48",
      url: "https://api.openweathermap.org/data/2.5/",
      query: '',
      codex: {},
    }
  },

  methods: {
    getWeather(e) {
      if(e.key == "Enter") {
        fetch(
          `${this.url}weather?q=${this.query}&units=metric&APPID=${this.key}`
        )
          .then((res) => {
            return res.json();
        })
          .then(this.setResult);
      }
    },

    setResult(result) {
      this.codex = result
      console.log(this.codex.weather[0].main)
    },

    getDate() {
      let d = new Date();
      let days = ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"];
      let months = ["January","February","March","April","May","June","July","August","September","October","November","December"];

      let date = d.getDate();
      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${date} ${month} ${year}, ${day}`;
    },
  }
}
</script>

<style>
* {
  margin: 0 !important;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  }

  #app.app {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-size: 220vh;
  background-repeat: no-repeat;
  color:white;
  background-image:url(./assets/sunny.jpg);
}

#app.clear {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-size: 220vh;
  background-repeat: no-repeat;
  color:white;
  background-image:url(./assets/sunny.jpg);
}

#app.cloudy {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-size: 220vh;
  background-repeat: no-repeat;
  color:white;
  background-image:url(./assets/cloudy.jpg);
}

#app.snowy {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  justify-content: center;
  height: 100vh;
  color:white;
  background-size: 220vh;
  background-repeat: no-repeat;
  background-image: url(./assets/snowy5.jpg);
} 

#app.rainy {
  display: flex;
  flex-direction: column;
  text-align: center;
  align-items: center;
  justify-content: center;
  height: 100vh;
  background-size: 220vh;
  background-repeat: no-repeat;
  color:white;
  background-image:url(./assets/rainy2.jpg);
}

.searchbar {
  width:50vh;
  border:none;
  border-bottom: 2px solid beige;
  background-color:rgba(228, 123, 14, 1);
  border-radius: 10px;
  padding: .6rem;
  color:white;
}

#description {
  text-transform: capitalize;
}

::placeholder {
  color:whitesmoke;
  padding: .3rem;
  font-size: 1rem;
}

input:focus {
  outline: none;
}

input:focus::placeholder {
  color: transparent
}

#location {
  padding-top: .5rem;
  font-size: 3rem;
}

#date {
  padding: 1rem;
  font-size: 1.3rem;
}

#temp {
  font-size:5rem;
  padding:.8rem;
}

#temp.hidden {
  visibility: hidden;
}

#location.hidden {
  display:none;
}


</style>
