<template>
  <div class="animated-number">
    <AnimatedBigNumber
      v-if="isBiggerOrEqualMillion"
      :initial-number="quotient"
    />

    <span
      class="animated-number__integer-part"
      :style="{
        '--number': isBiggerOrEqualMillion ? modulo : animatedNumber,
        '--transition': transition,
      }"
    />

    <span
      v-if="precision > 0"
      class="animated-number__decimal-divider"
    >{{ divider }}</span>

    <span
      v-if="precision > 0"
      class="animated-number__decimal-part"
      :style="{ '--decimal': decimalPart, '--transition': transition }"
    />

    <slot />
  </div>
</template>

<script>
import AnimatedNumber from '../src/AnimatedNumber.vue';
// eslint-disable-next-line import/no-self-import
import AnimatedBigNumber from './AnimatedBigNumber.vue';

export default {
  name: 'AnimatedBigNumber',

  components: {
    AnimatedBigNumber,
  },

  mixins: [
    AnimatedNumber,
  ],

  computed: {
    isBiggerOrEqualMillion() {
      return this.animatedNumber >= 1000000;
    },

    modulo() {
      return this.animatedNumber % 1000000;
    },

    quotient() {
      return Math.floor(this.animatedNumber / 1000000);
    },
  },
};
</script>
