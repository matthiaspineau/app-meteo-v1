<template>
  <div class="wrapper__carousel">
      <OwlCarousel :mouseDrag="true" 
      :nav="false" 
      :dots="false">
        <div
          v-for="(item, index) in file.daily"
          :key="index"
          :data-id="index"
          @click="emitGoDay(index)"
          class="item"
          :class="{ active : index == 0 }"
        >
        <span class="item__day">{{ item.dt | toDayString(lang.days) }}</span>
        <span class="item__date">{{ item.dt | toDateNumeric(file.timezone) }}</span>
        </div>
      </OwlCarousel>  
  </div>
</template>

<script>
import OwlCarousel from 'vue-owl-carousel'



export default {
  name: "Carousel",
  props: {
    file: {
      type: Object,
      default: function () {
        return {}
      }
    },
    lang: {
      type: Object,
        default: function () {
        return { }
        }
    }
  },
  methods: {
    emitGoDay(index) {
      // document.querySelector('.item[data-id="'+this.lastIndex+'"]').classList.remove('active')
      // e.target.classList.add('active')
      this.$emit("emit-go-day", { message: index});
    },
  },
  /**
   * get day name in day of week
   * 
   * @param {number} timestamp
   * @param {array} tab
   * 
   * @return {string}
   */
  filters: {
    toDayString(timestamp, tab) {
      let date = new Date(timestamp * 1000); // pour obtenir le timeStamp en millisecondes
      let day = date.getUTCDay();
      return tab[day]
    },
    /**
     * helper 
     * 
     * @param {number} timestamp
     * 
     * @return {string}
     */
    toDateNumeric (timestamp, timezone) {
      let date = new Date((timestamp - timezone )* 1000);
      let jour = date.getUTCDate(); 
      let mois = date.getUTCMonth(); 
      mois = mois + 1
      mois = mois.toString().length > 1 ? mois.toString() : '0' + mois
      let result = jour +"/"+ mois
      return result
    },
  },
  components: {
    OwlCarousel
  }
};
</script>
