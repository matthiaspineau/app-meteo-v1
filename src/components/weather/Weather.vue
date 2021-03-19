<template>
    <div>
        <div class="top">
          <h2 class="title">Paris</h2>

          <div class="article">
              <div class="article__media">
                <img
                  :src="require('@/assets/icon_meteo/'+file.weather[0].icon+'.png')"
                  alt="image"
                />
              </div>
              <div class="article__text">
                <div class="desc">{{ file.weather[0].main }}</div>
                <div class="temp">{{  Math.round(file.temp.day) }} °C</div>
              </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Weather',
    props: {
        file: {
            type: Object,
            default: function () {
            return { }
            }
        }
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
        font-size: 3.5rem;
      }

    }

  }


}
</style>