<template>
    <div class="wrapper__waiting">
        <div class="wait">
            <div class="wait__text">
                <h1>OpenWeather</h1>
                <p>{{ lang }}</p>
            </div>
                <div class="wait__media">
                <img class="sm__icon__meteo"
                :src="require('@/assets/icon_meteo/01d.png')"
                alt="image"
                />

            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Waiting',
    props: {
         lang: {
            type: Object,
                default: function () {
                return { }
            }
        }
    }
}
</script>

<style lang="scss" scoped>
.wrapper__waiting {
    min-height: 500px;
    margin: 10px;
    
   
}

.wait {
    display: flex;
    padding-top: 60px;

    &__media {

        .sm__icon__meteo {
            max-width: 150px;
        }
  
    }

    &__text {
        padding-right: 15px;

        h1 {
            font-size: 2rem;
            
        }

        p {
            max-width: 200px;
        }
    }
}
</style>
