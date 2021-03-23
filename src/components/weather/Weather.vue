<template>
<transition name="fade" appear>
    <div class="section">
        <div class="top">
          <h2 class="title">{{ city }}</h2>

          <div class="article">
              <div class="article__media">
                <img
                  :src="require('@/assets/icon_meteo/'+file.weather[0].icon+'.png')"
                  alt="image"
                />
              </div>
              <div class="article__text">
                <div class="desc">{{ file.weather[0].description }}</div>
                <div class="temp">{{  Math.round(file.temp.day) }}°</div>
              </div>
            </div>
        </div>

        <div class="wrapper__table">
        <div class="table">
          <div class="table__card">
            <SvgMinTemp :color="'#000'"></SvgMinTemp>
            <span class="valeur">{{ file.temp.min }} °</span>
            <span class="small">{{ lang.table.min }}</span>
          </div>
          <div class="table__card">
            <SvgMaxTemp :color="'#000'"></SvgMaxTemp>
            <span class="valeur">{{ file.temp.max }} °</span>
            <span class="small">{{ lang.table.max }}</span>
          </div>
          <div class="table__card">
            <SvgBarometer :color="'#000'"></SvgBarometer>
            <span class="valeur">{{ file.pressure }}</span>
            <span class="small">{{ lang.table.pressure }}</span>
          </div>

          <div class="table__card">
            <SvgDrop :color="'#000'"></SvgDrop>
            <span class="valeur">{{ file.humidity}} %</span>
            <span class="small">{{ lang.table.humidity }}</span>
          </div>

          <div class="table__card">
            <SvgWind :color="'#000'"></SvgWind>
            <span class="valeur">{{ file.wind_speed }} km/h</span>
            <span class="small">{{ lang.table.wind }}</span>
          </div>

          <div class="table__card">
            <SvgVisibility :color="'#000'"></SvgVisibility>
            <span class="valeur">{{ file.uvi }}</span>
            <span class="small">{{ lang.table.uvi }}</span>
          </div>

        </div>
      </div>
    </div>
</transition>
</template>

<script>
import SvgMaxTemp from '@/components/svg/SvgMaxTemp.vue';
import SvgMinTemp from '@/components/svg/SvgMinTemp.vue';
import SvgBarometer from '@/components/svg/SvgBarometer.vue';
import SvgDrop from '@/components/svg/SvgDrop.vue';
import SvgWind from '@/components/svg/SvgWind.vue';
import SvgVisibility from '@/components/svg/SvgVisibility.vue';

export default {
  name: 'Weather',
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
    },
    city: {
      type: String,
      default: 'test'
    }
  },
  components: {
    SvgMaxTemp,
    SvgMinTemp,
    SvgBarometer,
    SvgDrop,
    SvgWind,
    SvgVisibility
  }
}
</script>

<style lang="scss" scoped>
.top {

  .title {
    text-align: center;
  }

  .article {
    display: flex;

    &__media {
      width: 50%;
      padding: 5px;
    }

    &__text {
       width: 50%;
       padding: 5px;

      .desc {
        // color: green
      }

      .temp {
        // color: blue
        font-size: 4.5rem;
      }

    }

  }
}

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