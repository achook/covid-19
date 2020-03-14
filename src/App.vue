<template>
  <div id="app">
    <img class="virus-image" alt="Coronavirus visualization" src="./assets/coronavirus.png">
    <VirusStats/>
  </div>
</template>

<script>
import VirusStats from './components/VirusStats.vue'

export default {
    name: 'App',
    components: {
        VirusStats
    }
}
</script>

<style lang="scss">

html, body {
    height: 100%;
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.virus-image {
    width: auto;
    height: auto;
    max-height: 30%;
}

#app {
    height: 100%;
    box-sizing: border-box;

    padding: 60px;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    text-align: center;
    font-family:-apple-system, BlinkMacSystemFont, 'Helvetica Neue', 'Segoe UI', Roboto, Ubuntu, 'Open Sans', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

</style>
