<template>
<!--Fazen a alteração da foto do background para quando a temperatura for maior que 24 | <=24=frio / =>24=calor-->
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 24 ? 'warm' : ''">
    <main>
    <!--Caixa de pesquisa-->
      <div class="search-box">
        <input 
          type="text" 
          class="search" 
          placeholder="Pesquisar cidade..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>
    
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
    <!--Mostra o nome da cidade que foi pesquisada e a data atual-->
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

    <!--Mostra o resultado da temperatura da localidade pesquisada-->
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return {
      api_key: '7dd4c863fd02ccab4a103e469fc61d81', // Chave Key gerada no site OpenWeatherMap
      url_base: 'https://api.openweathermap.org/data/2.5/', // URL utilizada pela chave key para fazer a solicitação
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") { // Só fará a busca pela cidade ao clicar em ENTER
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () { // Criando critérios especificos para data
      let d = new Date();
      let months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
      let days = ["Domingo", "Segunda-feira", "Terça-feira", "Quarta-feira", "Quinta-feira", "Sexta-feira", "Sábado"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  background-image: url('./assets/f1.jpg');
  background-size: cover;
  background-position: flex;
  transition: 0.4s;
}

#app.warm {
  background-image: url('./assets/f2.jpg');
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to flex, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75));
}

.search-box {
  width: 50%;
  margin-left: 25%; 
  margin-bottom: 30px;
}

.search-box .search {
  width: 100%; 
  text-align: left; 
  margin-left: auto; 
  margin-right: auto;
  display: block;
  padding: 15px;
  cursor: pointer;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 5px 5px 5px 5px;
  transition: 0.4s;
}

.search-box .search:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 16px 16px 16px;
}

.location-box .location {
  color: #FFF;
  font-family: 'Summit', monospace;
  font-size: 45px;
  font-weight: 800;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: #FFF;
  font-family: 'Calendas Plus', sans-serif;
  font-style: italic;
  font-size: 20px;
  font-weight: 300;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #FFF;
  font-family: 'Intro', sans-serif;
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color:rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #FFF;
  font-family: 'Intro', sans-serif;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

</style>
