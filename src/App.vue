<template>
  <div id="app">
    <system-graphic></system-graphic>
  </div>
</template>

<script>
import SystemGraphic from './components/SystemGraphic.vue';

export default {
  name: 'app',
  components: {
    SystemGraphic
  }
};
</script>

<style lang="scss">
  * {
    margin: 0;
    padding: 0;
  }
</style>
