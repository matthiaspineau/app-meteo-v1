<template>
  <div id="app" class="development">
    <!-- Navbar -->
    <nav class="navbar">
      <h1>Méteo</h1>
    </nav>

    <!-- <div>{{ (1616064213+ 3600)  |toDateFull() }}</div> -->

    <!-- Formulaire -->
    <div class="d-flex mt-2">
      <div>
        <input v-model.lazy="city" 
          type="text"
          placeholder="entrer votre ville">
      </div>
      <div>
        <button @click="getWeathers(formatCity(city))">cliquer</button>
      </div>
    </div>

    <div>
      <Carousel @emit-go-day="goDay"></Carousel>
    </div>

    <main>

      <section class="section">
    
        <Weather :file="localFile.weathers.daily[idDay]"></Weather>

      </section>

    </main>

    <Home></Home>

  </div>
</template>

<script>
import Axios from 'axios';
import full from '@/assets/full.json'
import Carousel from '@/components/carousel/Carousel.vue'
import Weather from '@/components/weather/Weather.vue';
import Home from '@/components/Home.vue';
// import LoadingWeather from '@/components/weather/LoadingWeather.vue'
// import Table from '@/components/table/Table.vue'
// const ApiKey = '728e0adecbfd917c5d6a91e18904d29b'
const coord = [
    {
        "name": "Paris",
        "local_names": {
            "af": "Parys",
            "ar": "باريس",
            "ascii": "Paris",
            "bg": "Париж",
            "ca": "París",
            "da": "Paris",
            "de": "Paris",
            "el": "Παρίσι",
            "en": "Paris",
            "eu": "Paris",
            "fa": "پاریس",
            "feature_name": "Paris",
            "fi": "Pariisi",
            "fr": "Paris",
            "gl": "París",
            "he": "פריז",
            "hi": "पैरिस",
            "hr": "Pariz",
            "hu": "Párizs",
            "id": "Paris",
            "it": "Parigi",
            "ja": "パリ",
            "la": "Lutetia Parisorum",
            "lt": "Paryžius",
            "mk": "Париз",
            "nl": "Parijs",
            "no": "Paris",
            "pl": "Paryż",
            "pt": "Paris",
            "ro": "Paris",
            "ru": "Париж",
            "sk": "Paríž",
            "sl": "Pariz",
            "sr": "Париз",
            "th": "ปารีส",
            "tr": "Paris",
            "vi": "Paris"
        },
        "lat": 48.8534,
        "lon": 2.3488,
        "country": "FR"
    }
]

export default {
  name: "App",
  data() {
    return {
      localFile: {
        coordone: coord,
        weathers: full
      },
      api: {
        key: '728e0adecbfd917c5d6a91e18904d29b',
        urlGeoPoint: 'http://api.openweathermap.org/geo/1.0/direct?',
        urlWeatherDays: 'https://api.openweathermap.org/data/2.5/onecall?exclude=minutely,hourly'
      },
      city: '',
      resultat: '',
      reponseApi: true,
      idDay: 0,   
    };
  },
  computed: { 
  },
  methods: {
    /**
     * Set index of day selected when click of item carousel
     * 
     * @param {object} payload
     */
    goDay(payload) {
      console.log(payload.message)
      this.idDay = payload.message
    },
    /**
     * Get weathers few days data of city
     * 
     * @param {string} city
     * 
     * @return {object}
     */
    getWeathers(city) {
      Axios.get(this.api.urlGeoPoint + 'q='+city+'&appid='+ this.api.key)
            .then((response) => {
              if (response.data.length >= 1) {
                return Axios.get(this.api.urlWeatherDays + '&lat='+response.data[0].lat+'&lon='+response.data[0].lon+'&appid='+ this.api.key)
              } 
              })
            .then((response) => {
              this.resultat = response.data
              this.reponseApi = true
            })
            .catch((error) => console.log(error))
    },
    /**
     * Get geocoding data of city
     * 
     * @param {string} city
     * 
     * @return {Array}
     */
    getCoord(city) {
      Axios.get(this.api.urlGeoPoint + 'q='+city+'&appid='+ this.api.key)
            .then((response) => {
                if (response.status == 200) {
                  console.log(response)
                  console.log(response.data)
                }
              })
            .catch((error) => console.log(error))
    },
    /**
     * Get weather data of city 7 days
     * 
     * @param {string} lat
     * @param {string} lon
     * 
     * @return {object}
     */
    getWeatherDays(lat, lon) {
      Axios.get(this.api.urlWeatherDays + '&lat='+lat+'&lon='+lon+'&appid='+ this.api.key)
            .then((response) => {
                if (response.status == 200) {
                  console.log(response.data)
                }
              })
            .catch((error) => console.log(error))
    },
    /**
     * helper 
     * 
     * @param {string} city
     * 
     * @return {string}
     */
    formatCity(city) {
      return city.replace(/ /g,'-').toLowerCase()
    }
  },
  filters: {
    /**
     * helper 
     * 
     * @param {number} timestamp
     * 
     * @return {string}
     */
    toDayString(timestamp) {
      let date = new Date(timestamp * 1000); // pour obtenir le timeStamp en millisecondes
      let day = date.getDay(); 
      let result = day
      return result
    },
    /**
     * helper 
     * 
     * @param {number} timestamp
     * 
     * @return {string}
     */
    toDateFull (timestamp) {
      // var timeStamp = 1345902217, // le TimeStamp à convertir
      let date = new Date(timestamp * 1000) ; // pour obtenir le timeStamp en millisecondes

      let annee = date.getUTCFullYear(); // on récupère l'année
      let jour = date.getUTCDate(); // on récupère le jour
      let mois = date.getUTCMonth(); // on récupère le nombre du mois (entre 0 et 11)
      let heure = date.getUTCHours(); // on récupère l'heure
      let min = date.getUTCMinutes(); // on récupère les minutes
      let sec = date.getUTCSeconds(); // on récupère les secondes
      mois = mois + 1
      mois = mois.toString().length > 1 ? mois.toString() : '0' + mois
      heure = heure.toString().length > 1 ? heure.toString() : '0' + heure
      min = min.toString().length > 1 ? min.toString() : '0' + min
      let result = "le "+jour+"/"+mois+"/"+annee+" - "+heure+"h "+min+"min "+sec+"s"
      // console.log(result); // affichera : le 25/08/2012 - 15h 43min 37s
      return result
    },
  },
  components: {
    // Table,
    Carousel,
    Weather,
    // LoadingWeather
    Home
  },
};
</script>

<style src="@/assets/scss/app.scss" lang="scss"></style>
<style lang="scss">
.wrapper__response {
  margin: 10px;
  padding: 15px;
  border: 1px solid black;
}
</style>
