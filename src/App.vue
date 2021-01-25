<template>
    <Colors-steps />
</template>

<script>
import ColorsSteps from './components/ColorsSteps.vue'

export default {
    name: 'App',
    components: {
        ColorsSteps,
    },
}
</script>

<style>
*,
*::before,
*::after {
    box-sizing: border-box;
}

html {
    font-size: 62.5%;

    @media only screen and (max-width: 900px) {
        font-size: 58%;
    }
}

body {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}
</style>
