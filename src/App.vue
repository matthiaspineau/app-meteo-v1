<template>
  <div id="app" class="">
    <!-- Navbar -->
    <nav class="navbar">
      <h1>Méteo</h1>
       <div class="wrapper__lang">
        <select  v-model="selectLang">
          <option value="fr">Fr</option>
          <option value="en">En</option>
        </select>
      </div>
    </nav>

    <!-- <div>{{ (1616064213+ 3600)  |toDateFull() }}</div> -->

    <!-- Formulaire -->
    <div class="wrapper__form">
      <div class="form">
        <input 
        :class="errorCity ? 'error' : '' " 
        type="text" v-model.lazy="city" 
        :placeholder="lang.form.placeholder" />
        <span v-if="errorCity" class="error__text">{{ lang.form.errorText }}</span>
      </div>
      <div>
        <button class="btn__call"
        @click="getWeathers(city)">{{ lang.form.btnSearch }}</button>
      </div>
    </div>

    <!-- Main -->
    <main>

      <div v-if="showResult">
        
        <!-- Carousel -->
        <Carousel @emit-go-day="goDay" :file="dtDays" :lang="lang"></Carousel>

       
        <!-- Weather -->
        <Weather :file="dataWeathers.daily[idDay]" :lang="lang" :city="citySelect"></Weather>

      
        <!-- Sun -->
        <Sun :file="dataWeathers.daily[idDay]" :lang="lang.sun" :timezone="timezone"></Sun>

      </div>
      <div v-else>
        <Waiting  :lang="lang.waiting"></Waiting>
      </div>

    </main>


  </div>
</template>

<script>
import Axios from 'axios';
import full from '@/assets/full.json'
import langFr from "@/assets/lang/fr.json";
import langEn from "@/assets/lang/en.json";
import Carousel from '@/components/carousel/Carousel.vue'
import Weather from '@/components/weather/Weather.vue';
import Waiting from '@/components/Waiting.vue';
import Sun from '@/components/sun/Sun.vue';

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
      devConfig: {
        useDevLocalFile: false
      },
      localFile: {
        coordone: coord,
        weathers: full
      },
      api: {
        key: '728e0adecbfd917c5d6a91e18904d29b',
        urlGeoPoint: 'http://api.openweathermap.org/geo/1.0/direct?',
        urlWeatherDays: 'https://api.openweathermap.org/data/2.5/onecall?exclude=minutely,hourly'
      },
      showResult: false,
      city: '',
      dataWeathers: '',
      idDay: 0,
      dtDays: {},
      carouselItem: {
        eltActive: 0,
        eltLast: 0
      },
      errorCity: false,
      selectLang: 'fr',
      messageErrorAPi: '',
      timezone: '' 
    };
  },
    computed: {
    lang() {
      if (this.showResult) {
        console.log('requete du au changement de langue')
        this.getWeathers(this.city)
      }
      return this.selectLang == 'fr' ? langFr.lang : langEn.lang
    },
    
  },
  methods: {
    /**
     * IN DEVELOPMENT
     * Get file full.json
     * 
     */
    getLocalFileFull() {
      this.dataWeather = full
      this.reqDtDays()
      this.showResult = true
    },
    /**
     * Create Array with dt days 
     * 
     * @param {Object}
     * 
     * @return {Object}
     */
    reqDtDays(file) {
      let timezone = file.timezone_offset
      let daily = []
      file.daily.forEach((element) => {
        daily.push({ "dt" : element.dt })
      });
      this.dtDays = {"timezone" : timezone, "daily": daily}
    },
    /**
     * Set index of day selected when click of item carousel
     * 
     * @param {object} payload
     */
    goDay(payload) {
      // console.log(payload)
      this.carouselItem.eltActive = payload.message
      document.querySelector('.item[data-id="'+this.carouselItem.eltActive+'"]').classList.add('active')
      document.querySelector('.item[data-id="'+this.carouselItem.eltLast+'"]').classList.remove('active')
      this.carouselItem.eltLast = payload.message
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
      let citie = this.formatCity(city)
      if (this.devConfig.useDevLocalFile) {
        this.dataWeathers = full
        this.citySelect = coord[0].name
        this.reqDtDays(this.dataWeathers)
        this.errorCity = false
        this.showResult = true
        this.timezone = this.dataWeathers.timezone_offset
      } else {
        Axios.get(this.api.urlGeoPoint+'q='+citie+'&appid='+this.api.key)
            .then((response) => {
              // console.log('requete : 1 , coordonnee')
              // console.log(response)
              if (response.data.length >= 1) {
                this.citySelect = response.data[0].local_names.eu
                return Axios.get(this.api.urlWeatherDays +'&lat='+response.data[0].lat+'&lon='+response.data[0].lon+'&exclude=minutely,hourly&lang='+this.selectLang+'&units=metric&appid='+this.api.key)
              }  else {
                this.errorCity = true
              }
              })
            .then((response) => {
              // console.log('requete : 2 , meteo')
              // console.log(response)
              this.dataWeathers = response.data
              this.reqDtDays(this.dataWeathers)
              this.timezone = this.dataWeathers.timezone_offset
              this.showResult = true
              this.errorCity = false
            })
             .catch((error) => {
              // console.log(error.response);
              this.messageErrorAPi = error.response
              this.errorCity = true
            })
      }
      
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
     * helper : format string
     * 
     * @param {string} city
     * 
     * @return {string}
     */
    formatCity(chaine) {
      let city = chaine.trim()
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
    toDayString(timestamp, timezone) {
      let date = new Date((timestamp + timezone) * 1000); // pour obtenir le timeStamp en millisecondes
      let day = date.getUTCDay();
      return day
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
    Carousel,
    Weather,
    Waiting,
    Sun
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
