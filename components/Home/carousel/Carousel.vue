<template>
  <div ref="carousel" class="carousel">
    <CarouselItem v-for="(ele, i) in elements" :key="i">ele</CarouselItem>
  </div>
</template>

<script>
import { defineComponent } from '@vue/composition-api';
import { CarouselItem } from './CarouselItem';

export default defineComponent({
  components: {
    CarouselItem
  },
  data() {
    return {
      elements: this.$slots.default
    };
  },
  mounted() {
    this.arrange(0);
  },
  methods: {
    _mod: (n, m) => ((n % m) + m) % m,
    arrange(centerIndex) {
      const center = this.$children[centerIndex];
      const half = (this.$children.length - 1) / 2;
      const before = [];
      for (let i = centerIndex - 1; before.length < half; i--) {
        before.push(this.$children[this._mod(i, this.$children.length)]);
      }

      const after = [];
      for (
        let i = centerIndex + 1;
        after.length < this.$children.length - before.length - 1;
        i++
      ) {
        after.push(this.$children[this._mod(i, this.$children.length)]);
      }
      center.opacity = 1;
      center.zIndex = Math.max(before.length, after.length) + 1;
      [before, after].forEach((list, listIndex) => {
        let parentTrans = 0;
        list.forEach((item, i) => {
          item.scale = 0.8 ** (i + 1);
          const absolute = 105 * item.scale * 1.125 + parentTrans;
          parentTrans = absolute;
          item.xtrans = (listIndex === 0 ? -1 : 1) * absolute;
          item.opacity = Math.max(1 - 0.25 * (i / 2 + 1) ** 2, 0);
          item.zIndex = Math.max(before.length, after.length) - i;
        });
      });
    }
  }
});
</script>

<style>
.carousel {
  background-color: #ddd;
  height: 100vh;
  position: relative;
}
.carousel > div {
  background-color: #000;
  color: #fff;
  width: 10vw;
  height: 10vw;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  line-height: 10vw;
  font-family: sans-serif;
  font-size: 6vw;
}
.carousel .dots {
  position: absolute;
  bottom: 0;
  padding: 5%;
  width: 100%;
}
.carousel .dots .dot {
  background-color: #000;
}
</style>
