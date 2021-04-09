<template>
  <div id="app">
    <GisLeaflet />
  </div>
</template>

<script>

import GisLeaflet from './components/leaflet/example/Huli';

export default {
  name: 'App',
  components: {
    GisLeaflet
  }
}
</script>

<style>
#app {
  padding: 0;
  margin: 0;
  height: 100%;
  width: 100%;
}
</style>
