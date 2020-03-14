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

html {
    background-color: #fdfdfd;
    color: #020202;
}

html, body {
    box-sizing: border-box;
    height: 100%;

    margin: 0;
    padding: 0;

}

.virus-image {
    width: auto;
    height: auto;
    max-height: 30%;
}

#app {
    box-sizing: border-box;
    height: 100%;

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
