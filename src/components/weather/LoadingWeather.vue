<template>
    <div>
        <div class="loading-top">
          <div class="loading-title"></div>

          <div class="loading-article">
              <div class="loading-article__media">
                  <div class="img__circle"></div>
              </div>
              <div class="loading-article__text">
                <div class="desc"></div>
                <div class="temp"></div>
              </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'LoadingWeather',
}
</script>

<style lang="scss" scoped>
.loading-top {
    
  .loading-title {
    width: 100px;
    height: 1.2rem;
    margin: 0 auto;
    border-radius: 3px;
    background: rgb(95, 95, 95);
  }

  .loading-article {
    display: flex;
    margin-top: 25px;

    &__media {
      width: 50%;
      padding: 5px;

      .img__circle {
          width: 150px;
          height: 150px;
          border-radius: 50%;
          background: rgb(95, 95, 95);
      }
    }

    &__text {
        margin-top: 15px;
       width: 50%;
       padding: 5px;

      .desc {
        height: 1rem;
        background: rgb(95, 95, 95);
        line-height: 1rem;
        border-radius: 3px;
      }

      .temp {
        margin-top: 10px;
        height: 3.5rem;
        line-height: 3.5rem;
        font-size: 3.5rem;
        border-radius: 3px;
        background: rgb(95, 95, 95);
      }

    }

  }


}
</style>