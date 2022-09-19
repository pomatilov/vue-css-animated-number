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
import AnimatedNumber from '../src/AnimatedNumber.vue';

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

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding: 60px;

  margin: 0 auto;
  max-width: 300px;
  display: flex;
  flex-direction: column;

  button {
    margin-top: 2em;
  }
}
</style>
