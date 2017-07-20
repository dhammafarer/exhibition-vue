<template>
  <div class="tooltip">
    {{content}}
    <button @click="hideTooltip">close</button>
  </div>
</template>

<script>
export default {
  name: 'tooltip',
  props: {
    content: {
      default: 'tooltip...'
    }
  },
  methods: {
    hideTooltip () {
      this.$emit('hideTooltip')
    }
  }
}
</script>

<style>
  .tooltip {
    position: absolute;
    top: 50%;
    left: 50%;
    display: inline-block;
    padding: 20px;
    width: 200px;
    height: 150px;
    background: white;
    border-radius: 4px;
    border: 1px solid lightgrey;
  }
</style>
