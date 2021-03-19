<template>
    <div class="wrapper__carousel">
      <div class="carousel" v-touch:swipe="touchHandler">
        <div class="item">item</div>
        <div class="item">item</div>
        <div class="item">item</div>
        <div class="item">item</div>
        <div class="item">item</div>
        <div class="item">item</div>
        <div class="item">item</div>
      </div>
    </div>
</template>

<script>


export default {
  name: "Home",
  methods: {
    touchHandler() {
      console.log("ok");
    },
  
  },
};
</script>

<style lang="scss" scoped>
.wrapper__carousel {
  max-width: 200px;
  margin: 0 auto;
  padding-left: 25px;
  padding-right: 25px;
  margin-top: 15px;
  margin-bottom: 15px;
  overflow-x: hidden;
  border: 1px solid black;
}

.carousel {
  display: flex;
  width: 100%;

  .item {
    width: 165px;
    padding: 8px;
    margin: 5px;
    border-radius: 5px;
    border: 1px solid #000;
  }
}
</style>