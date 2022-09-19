<template>
  <div class="animated-number">
    <span
      class="animated-number__integer-part"
      :style="{ '--css-an-number': animatedNumber, '--css-an-transition': transition }"
    />

    <span
      v-if="precision > 0"
      class="animated-number__decimal-divider"
    >{{ divider }}</span>

    <span
      v-if="precision > 0"
      class="animated-number__decimal-part"
      :style="{ '--css-an-decimal': decimalPart, '--css-an-transition': transition }"
    />

    <slot />
  </div>
</template>

<script>
export default {
  model: {
    prop: 'initialNumber',
    event: 'input',
  },

  props: {
    initialNumber: [Number, String],

    precision: {
      type: Number,
      default: 0,
    },

    divider: {
      type: String,
      default: '.',
    },

    transition: {
      type: String,
      default: '.4s',
    },

    roundValue: Boolean,
  },

  data: () => ({
    animatedNumber: 0,
    decimalPart: 0,
    isZeroDecimal: null,
  }),

  watch: {
    initialNumber: {
      immediate: true,
      handler(newVal) {
        let value = newVal;

        if (typeof newVal === typeof '') {
          value = Number(newVal);
        }

        if (Number.isNaN(value)) {
          throw new Error(`Invalid number: "${value}"`);
        }

        if (Number.isNaN(value) === false && Number.isFinite(value)) {
          if (this.precision > 0) {
            this.animatedNumber = value;

            const diff = this.animatedNumber - Math.floor(this.animatedNumber);

            const decimalPart = diff * (10 ** this.precision);

            this.decimalPart = Math.round(decimalPart);

            this.isZeroDecimal = this.decimalPart === 0;
          } else {
            this.animatedNumber = this.roundValue
              ? Math.round(value)
              : value;
          }
        }
      },
    },
  },
};
</script>

<style lang="css">
@property --css-an-number {
  syntax: "<number>";
  initial-value: 0;
  inherits: false;
}

@property --css-an-decimal {
  syntax: "<number>";
  initial-value: 0;
  inherits: false;
}
</style>

<style scoped lang="scss">
.animated-number {
  display: inline-block;

  * {
    display: inline-block;
  }

  .animated-number__integer-part {
    --css-an-integer-part: max(var(--css-an-number) - 0.5, 0);

    transition: --css-an-number var(--css-an-transition);

    counter-reset: css-an-integer-counter var(--css-an-integer-part);

    &::before {
      content: counter(css-an-integer-counter);
    }
  }

  .animated-number__decimal-part {
    --css-an-decimal-part: max(var(--css-an-decimal) - 0.5, 0);

    transition: --css-an-decimal var(--css-an-transition);

    counter-reset: css-an-decimal-counter var(--css-an-decimal-part);

    &::before {
      content: counter(css-an-decimal-counter, decimal-leading-zero);
    }
  }
}
</style>
