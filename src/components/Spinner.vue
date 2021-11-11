<template>
  <div class="loader">Loading...</div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';

@Options({
})
export default class Spinner extends Vue {}
</script>

<style scoped lang="scss">
  .loader,
  .loader:after {
    border-radius: 50%;
    width: 1.5em;
    height: 1.5em;
  }
  .loader {
    font-size: 10px;
    position: relative;
    text-indent: -9999em;
    border-top: 0.2em solid rgba(0, 0, 0, 0.2);
    border-right: 0.2em solid rgba(0, 0, 0, 0.2);
    border-bottom: 0.2em solid rgba(0, 0, 0, 0.2);
    border-left: 0.2em solid #000000;
    transform: translateZ(0);
    animation: load8 1.1s infinite linear;
    overflow: hidden;
  }

  @keyframes load8 {
    0% {
      -webkit-transform: rotate(0deg);
      transform: rotate(0deg);
    }
    100% {
      -webkit-transform: rotate(360deg);
      transform: rotate(360deg);
    }
  }

</style>
