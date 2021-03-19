<template>
    <div class="wrapper__table">
        <div class="table__top">
          <div class="table__card">
            <SvgMinTemp></SvgMinTemp>
            <span class="valeur">{{ file.temp.min }} °</span>
            <span class="small">{{ lang.min }}</span>
          </div>
          <div class="table__card">
            <SvgMaxTemp></SvgMaxTemp>
            <span class="valeur">{{ file.temp.max }} °</span>
            <span class="small">{{ lang.max }}</span>
          </div>
          <div class="table__card">
            <SvgBarometer></SvgBarometer>
            <span class="valeur">{{ file.pressure }}</span>
            <span class="small">{{ lang.pressure }}</span>
          </div>

          <div class="table__card">
            <SvgDrop></SvgDrop>
            <span class="valeur">{{ file.humidity }} %</span>
            <span class="small">{{ lang.humidity }}</span>
          </div>

          <div class="table__card">
            <SvgWind></SvgWind>
            <span class="valeur">{{ file.wind_speed }} km/h</span>
            <span class="small">{{ lang.wind }}</span>
          </div>

          <div class="table__card">
            <SvgVisibility></SvgVisibility>
            <span class="valeur">{{ file.uvi }} km</span>
            <span class="small">{{ lang.visibility }}</span>
          </div>

        </div>
      </div>
</template>

<script>
import SvgDrop from "@/components/svg/SvgDrop.vue";
import SvgWind from "@/components/svg/SvgWind.vue";
import SvgMinTemp from "@/components/svg/SvgMinTemp.vue";
import SvgMaxTemp from "@/components/svg/SvgMaxTemp.vue";
import SvgBarometer from "@/components/svg/SvgBarometer.vue";
import SvgVisibility from "@/components/svg/SvgVisibility.vue";

export default {
    name: 'Table',
    props: {
        file: {
            type: Object,
            default: function () {
            return { }
            }
        },
        lang: {
            type: Object,
            default: function () {
            return { }
            }
        }
    },
    filters: {
    toKm: function (value) {
      return value / 1000
    },
    toDate: function (timeStamp) {
      let date = new Date(timeStamp * 1000); 
      let heure = date.getHours(); 
      let min = date.getMinutes();

      return heure + ":" + min;
    },
  },
    components: {
        SvgDrop,
        SvgWind,
        SvgMinTemp,
        SvgMaxTemp,
        SvgBarometer,
        SvgVisibility,
    }
}
</script>


<style lang="scss" scoped>
.table {

    display: flex;
    flex-wrap: wrap;


    &__card {
        width: 33.33%;
        margin-top: 15px;
        padding: 5px;
        text-align: center;

        .valeur {
            display: block;
            margin-top: 10px;
        }

        .small {
            color: rgb(116, 116, 116);
            font-size: 0.8rem;
        }
    }
}
</style>