<template>
  <div></div>
</template>

<script>
import { defineComponent } from '@vue/composition-api';

export default defineComponent({
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
  },
  render(createElement) {
    return createElement(
      'div',
      {
        class: 'carousel',
        ref: 'carousel'
      },
      this.$slots.default.map((el) => createElement('carousel-item', [el]))
    );
  }
});
</script>
