<template>
    <div class="wrapper__carousel">


      <OwlCarousel :mouseDrag="true" :nav="false" :number="7" :dots="false">
        <div class="item">item 1</div>
        <div class="item">item 2</div>
        <div class="item">item 3</div>
        <div class="item">item 4</div>
        <div class="item">item 5</div>
        <div class="item">item 6</div>
        <div class="item">item 7</div>
      </OwlCarousel>
    </div>
</template>

<script>
import OwlCarousel from 'vue-owl-carousel'

export default {
  name: "Dev",
  methods: {
    touchHandler() {
      console.log("ok");
    },
  
  },
  components: {
    OwlCarousel
  }
};
</script>

<style lang="scss" scoped>

.item {
    // width: 165px;
    padding: 8px;
    margin: 5px;
    border-radius: 5px;
    border: 1px solid #000;
  }


</style>