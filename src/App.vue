<template>
  <div id="app">
    <QuickStartGuide />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import QuickStartGuide from './components/leaflet/example/QuickStartGuide.vue';

export default {
  name: 'App',
  components: {
    HelloWorld,
    QuickStartGuide
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
