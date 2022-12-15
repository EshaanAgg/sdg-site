<template>
  <div class="dots">
    <CarouselDot v-for="i in numDots" :key="i"></CarouselDot>
  </div>
</template>

<script>
import { defineComponent } from '@vue/composition-api';
import { CarouselDot } from './CarouselDot.vue';

export default defineComponent({
  components: {
    CarouselDot
  },
  props: { numDots: { type: Number, required: true } }
});
</script>
