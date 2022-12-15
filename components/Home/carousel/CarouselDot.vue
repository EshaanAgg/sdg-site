<template>
  <div class="dot" :style="style"></div>
</template>

<script>
import { defineComponent } from '@vue/composition-api';

export default defineComponent({
  data() {
    return {
      selected: false
    };
  },
  computed: {
    style: () => ({ opacity: this.selected ? 1 : 0.5 })
  }
});
</script>
