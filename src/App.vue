<template>
  <Dropdown />
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import Dropdown from './components/Dropdown/Dropdown.vue';

@Options({
  components: {
    Dropdown,
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
body {
  padding: 0;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
</style>
