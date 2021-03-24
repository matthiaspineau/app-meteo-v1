<template>
  <div class="section bg__sm">
    <div class="wrapper__sun section__sm">
      <div class="left" :class="{ orderLast: isNuit }">
        <span>{{ file.sunrise | toDateHM(timezone) }}</span>
        <small class="small">{{ lang.sunrise }}</small>
      </div>

      <div class="center">
        <span>{{ dateNow | toDateHM(timezone) }}</span>
        <!-- <div class="wrapper__dessin">
          <div class="dessin">
            <span
              class="sunSpan"
              :class="{ jour: isJour, nuit: isNuit }"
              :style="{ left: positionCercle + '%' }"
            ></span>
          </div>
        </div> -->
      </div>

      <div class="right" :class="{ orderFirst: isNuit }">
        <span>{{ file.sunset | toDateHM(timezone) }}</span>
        <small class="small">{{ lang.sunset }}</small>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Sun",
  props: {
    file: {
      type: Object,
      default: function () {
        return {};
      },
    },
    lang: {
      type: Object,
      default: function () {
        return {};
      },
    },
    timezone: {
      type: Number,
      default: 0,
    },
  },
  computed: {
    dateNow() {
      let date = Math.round(new Date().getTime() / 1000);
      return date;
    },
    positionCercle() {
      let deb = this.hourRound(this.file.sunrise + this.timezone);
      let act = this.hourRound(this.dateNow + this.timezone);
      let fin = this.hourRound(this.file.sunset + this.timezone);

      let result = (act * 100) / (deb + fin);

      // console.log(result)
      return result;
    },
    isJour() {
      let deb = this.file.sunrise;
      let act = this.file.dt;
      let fin = this.file.sunset;
      if (deb <= act && act <= fin) {
        // console.log('jour')
        return true;
      } else {
        // console.log('nuit')
        return false;
      }
    },
    isNuit() {
      let deb = this.file.sunrise;
      let act = this.file.dt;
      let fin = this.file.sunset;
      if (deb <= act && act <= fin) {
        // console.log('jour')
        return false;
      } else {
        // console.log('nuit')
        return true;
      }
    },
  },
  methods: {
    hourRound(timestamp) {
      let date = new Date(timestamp * 1000);
      let heure = date.getUTCHours();
      return Math.round(heure);
    },
  },
  filters: {
    toDateHM: function (timeStamp, timezone) {
      let date = new Date((timeStamp + timezone) * 1000);
      let heure = date.getUTCHours();
      let min = date.getUTCMinutes();
      heure = heure.toString().length > 1 ? heure.toString() : "0" + heure;
      min = min.toString().length > 1 ? min.toString() : "0" + min;
      return heure + ":" + min;
    },
  },
};
</script>

<style lang="scss" scoped>
.wrapper__dessin {
  // position: relative;
  margin: 0 auto;
  padding-bottom: 10px;
  padding-top: 10px;

  .dessin {
    position: relative;
    margin-top: 5px;
    width: 100%;
    height: 1px;
    background: rgb(116, 116, 116);

    .sunSpan {
      position: absolute;
      bottom: calc(0% - 10px);
      // right: calc(50% - 10px);
      width: 20px;
      height: 20px;
      border-radius: 50%;
      // border: 1px solid red;
    }

    .jour {
      background: rgb(231, 111, 31);
      border: 1px solid rgb(231, 111, 31);
    }

    .nuit {
      background: rgb(46, 46, 189);
      border: 1px solid rgb(46, 46, 189);
    }
  }
}

.wrapper__sun {
  display: flex;
  align-items: baseline;

  .center {
    width: 50%;
    padding: 5px;
    text-align: center;
    order: 2;
  }
  .left,
  .right {
    width: 25%;
    padding: 5px;
    text-align: center;

    span {
      display: block;
    }
  }

  .left {
    order: 1;
  }

  .right {
    order: 3;
  }

  .left.orderLast {
    order: 3;
  }

  .right.orderFirst {
    order: 1;
  }

  .small {
    margin-top: 5px;
    display: block;
    color: rgb(116, 116, 116);
    font-size: 0.8rem;
  }
}
</style>