<template>
  <div class="animated-number">
    <span
      class="animated-number__integer-part"
      :style="{ '--number': animatedNumber, '--transition': transition }"
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
@property --number {
  syntax: "<number>";
  initial-value: 0;
  inherits: false;
}

@property --decimal {
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
    --integer-part: max(var(--number) - 0.5, 0);

    transition: --number var(--transition);

    counter-reset: integer-part var(--integer-part);

    &::before {
      content: counter(integer-part);
    }
  }

  .animated-number__decimal-part {
    --decimal-part: max(var(--decimal) - 0.5, 0);

    transition: --decimal var(--transition);

    counter-reset: decimal-part var(--decimal-part);

    &::before {
      content: counter(decimal-part, decimal-leading-zero);
    }
  }
}
</style>
