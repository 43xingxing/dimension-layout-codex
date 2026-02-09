<template>
  <div id="app" class="app-container">
    <DimensionPanel />
  </div>
</template>

<script>
import DimensionPanel from './components/DimensionPanel.vue'

export default {
  name: 'App',
  components: {
    DimensionPanel
  }
}
</script>

<style>
html,
body,
#app {
  height: 100%;
  margin: 0;
}

.app-container {
  height: 100%;
  background: #f5f7fa;
  padding: 16px;
  box-sizing: border-box;
}
</style>
