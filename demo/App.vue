<template>
  <div id="app">
    <div class="container">
      <h1 class="header">
        Demo
      </h1>

      <h2>Usage</h2>

      <section>
        <label for="integerNumber">
          <input
            v-model="integerNumber"
            id="integerNumber"
            type="number"
          />

          <span>Integer number</span>
        </label>

        <AnimatedNumber
          v-model="integerNumber"
          class="animated-num"
        />
      </section>

      <section>
        <label for="roundedIntegerNumber">
          <input
            v-model="roundedIntegerNumber"
            id="roundedIntegerNumber"
            type="number"
          />

          <span>Rounded integer number</span>
        </label>

        <AnimatedNumber
          v-model="roundedIntegerNumber"
          class="animated-num"
          round-value
        />
      </section>

      <section>
        <label for="decimalNumber">
          <input
            v-model="decimalNumber"
            id="decimalNumber"
            type="number"
          />

          <span>Decimal number with precision = 2</span>
        </label>

        <AnimatedNumber
          v-model="decimalNumber"
          class="animated-num"
          :precision="2"
        />
      </section>

      <h2>With slot</h2>

      <section>
        <label for="priceWithoutDiscount">
          <input
            v-model="priceWithoutDiscount"
            id="priceWithoutDiscount"
            type="number"
          />

          <span>Price without discount</span>
        </label>

        <label for="priceWithDiscount">
          <input
            v-model="priceWithDiscount"
            id="priceWithDiscount"
            type="money"
          />

          <span>Price with discount</span>
        </label>

        <div class="price">
          <AnimatedNumber
            v-model="priceWithoutDiscount"
            class="animated-num common-price"
            :precision="2"
            round-value
          >
            <AnimatedNumber
              v-model="priceWithDiscount"
              class="animated-num discount-price"
              :precision="2"
              round-value
            />
          </AnimatedNumber>
        </div>
      </section>

      <h2>Special</h2>

      <section>
        <label for="bigIntegerNumber">
          <input
            v-model="bigIntegerNumber"
            id="bigIntegerNumber"
            type="number"
          />

          <span style="line-height:1.75">
            Big integer number (self-used component)<br/>
            Maximum number = MAX_SAFE_INTEGER<br/>
            Check AnimatedBigNumber.vue in /examples folder<br/>
          </span>
        </label>

        <AnimatedBigNumber
          v-model="bigIntegerNumber"
          class="animated-num"
        />
      </section>
    </div>
  </div>
</template>

<script>
import AnimatedNumber from '../src/AnimatedNumber.vue';
import AnimatedBigNumber from '../examples/AnimatedBigNumber.vue';

export default {
  name: 'App',

  components: {
    AnimatedNumber,

    AnimatedBigNumber,
  },

  data: () => ({
    integerNumber: 0,
    roundedIntegerNumber: 0,
    decimalNumber: 0,
    roundedDecimalNumber: 0,

    priceWithoutDiscount: 0,
    priceWithDiscount: 0,

    bigIntegerNumber: 0,
  }),

  mounted() {
    const vm = this;
    setTimeout(() => {
      vm.integerNumber = 520;
      vm.roundedIntegerNumber = 49.85;
      vm.decimalNumber = 340.689;
      vm.roundedDecimalNumber = 147.0;

      vm.priceWithoutDiscount = 162.54;
      vm.priceWithDiscount = 130.25;

      vm.bigIntegerNumber = Number.MAX_SAFE_INTEGER;
    }, 1000);
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 60px;
}

.container {
  max-width: 320px;
  margin: 0 auto;
}

h2 {
  margin: 2em 0;
}

label {
  display: flex;
  flex-direction: column-reverse;
  margin-top: 2em;
}

label > span {
  margin-bottom: .5em;
  text-align: left;
}

.animated-num {
  margin-top: 1em;
  font-weight: bold;
}

.price {
  font-size: 24px;
  letter-spacing: 6px;
  text-decoration: inherit;

  &::before {
    margin-right: .25em;
    color: black;
    font-weight: bold;
    content: "$";
  }

  .animated-num.common-price {
    color: grey;
    text-decoration: line-through;

    & > span {
      text-decoration: inherit;
    }
  }

  .animated-num.discount-price {
    margin-left: .5em;
    color: black;
    text-decoration: none;

    & > span {
      text-decoration: inherit;
    }
  }
}

</style>
