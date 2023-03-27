# vue-css-animated-number

A simple component that animate number using [CSS counters](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Counter_Styles/Using_CSS_counters) and [@property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)

Notes:

1. **@property has problems with compatibility**. Check browser compatibility on [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@property#browser_compatibility)
2. **Maximum integer is 999 999**, cause after that variables uses exponent. Sometimes it can broke animation, but number can be extended to MAX_SAFE_INTEGER with self-imported component (check /examples/AnimatedBigNumber.vue) 

## Usage

```vue
<template>
  <div id="app">
    <AnimatedNumber
      v-model="decimalNumber"
      class="animated-number"
      :precision="2"
    />

    <button type="button" @click="onButtonSetNewValueClick">
      Set new value
    </button>
  </div>
</template>

<script>
import AnimatedNumber from '...';

export default {
  components: {
    AnimatedNumber,
  },

  data: () => ({
    decimalNumber: 333,
  }),

  methods: {
    onButtonSetNewValueClick() {
      this.decimalNumber = Math.random() * 1000;
    },
  },
};
</script>

<style src=".../vue-css-animated-number.css">
</style>
```

## Props

| Prop name | Type | Description | Default | Required |
|-----|-----|-----|-----|-----|
|initialNumber|[Number,String]|Value that will be animated||Y|
|divider|String|Divider between integer and decimal parts|"."|N|
|precision|Number|Count of digits, which will be presented|0|N|
|roundValue|Boolean|Rounding value (works if precision = 0, if > 0 - rounding automatically to avoid math`s problems)|false|N|
|transition|String|Transition property for animation|".4s"|N|
