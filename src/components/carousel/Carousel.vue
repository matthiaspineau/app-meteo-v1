<template>
  <div>

    <div class="wrapper__carousel">
      <div class="carousel">
        <div
          v-for="(item, index) in days"
          :key="index"
          :data-id="index"
          @click="emitGoDay(index)"
          class="item"
        >
          {{ item }}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Carousel",
  props: {
    days: {
      type: Number,
      default: 7,
    },
  },
  methods: {
    emitGoDay(index) {
      this.$emit("emit-go-day", { message: index });
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