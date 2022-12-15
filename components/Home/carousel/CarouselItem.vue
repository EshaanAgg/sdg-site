<template>
  <div class="carousel-item" :style="style" @click="centerSelf">
    <slot></slot>
  </div>
</template>

<script>
import { defineComponent } from '@vue/composition-api';

export default defineComponent({
  data: () => ({ zIndex: 0, xtrans: 0, ytrans: 0, scale: 1, opacity: 1 }),
  computed: {
    style() {
      return {
        transition: 'transform 0.5s, opacity 0.5s',
        transform: this.transform,
        'z-index': this.zIndex,
        opacity: this.opacity
      };
    },
    transform() {
      return [
        `translate(${this.xtrans - 50}%, ${this.ytrans - 50}%)`,
        `scale(${this.scale})`
      ].join(' ');
    }
  },
  methods: {
    centerSelf() {
      this.$parent.arrange(this.$parent.$children.indexOf(this));
    }
  }
});
</script>
