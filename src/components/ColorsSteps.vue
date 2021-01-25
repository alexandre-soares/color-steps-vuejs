<template>
    <div class="app">
        <div class="tools">
            <div class="tools__first-color">
                <input type="color" v-model="color1" />
                <input type="text" v-model="color1" />
            </div>
            <div class="tools__range">
                <input
                    type="range"
                    :min="min"
                    :max="max"
                    v-model="steps"
                    class="tools__range-input"
                />
                <div class="tools__steps">{{ steps }} Steps</div>
            </div>
            <div class="tools__second-color">
                <input type="color" v-model="color2" />
                <input type="text" v-model="color2" />
            </div>
        </div>
        <div class="colors"></div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            min: 3,
            max: 25,
            steps: 5,
            color1: '#FD6B42',
            color2: '#E342FD',
        }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$fg: #fff;
$bg: #000;

.app {
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.tools {
    padding: 4rem 2rem;
    font-size: 4rem;
    width: 80vw;
    display: flex;
    justify-content: space-around;
    align-items: center;

    &__first-color,
    &__second-color {
        display: flex;
        align-items: initial;
    }

    &__range {
        flex: 1;
        text-align: center;
        display: flex;
        flex-direction: column;
        align-items: center;
        transform: translateY(30%);
    }

    &__steps {
        margin-top: 2rem;
    }
}

input {
    margin: 0;
    padding: 0 1rem 0 1rem;
    font-size: inherit;
    font-family: inherit;
    border: none;
    width: 22rem;
    outline: none;
    border-radius: 0 1em 1em 0;
    background-color: #eee;
    cursor: pointer;

    &[type='color'] {
        padding: 0;
        width: 2em;
        height: 2em;
        border-radius: 1em 0 0 1em;

        &::-moz-color-swatch {
            border: none;
            border-radius: 1em;
            transform: scale(0.75);
        }

        &::-webkit-color-swatch {
            border: none;
            border-radius: 1em;
        }
        &::-webkit-color-swatch-wrapper {
            padding: 0.25em;
            border-radius: 1em;
        }
    }

    &[type='range'] {
        padding: 0;
    }

    &[type='range'] {
        appearance: none;
        outline: none;
        display: inline-block;
        padding: 0;
        margin: 0;
        border: 0;
        height: 0.25rem;
        border-radius: 1rem;
        cursor: pointer;
        font-size: inherit;
        background: $bg;

        // MOZILLA
        &::-moz-range-track {
            appearance: none;
            background: $bg;
            outline: none;
        }
        &::-moz-focus-outer {
            border: 0;
        }
        &::-moz-range-thumb {
            appearance: none;
            width: 0.1rem;
            height: 0.1rem;
            border: none;
            border-radius: 1em;
            box-shadow: 0 0 0 0.3em $bg;
            background: $bg;
            transform: scale(1);
            transition: transform 0.3s ease-out;
        }
        &::-moz-range-thumb:focus,
        &::-moz-range-thumb:active {
            appearance: none;
            transform: scale(0.85);
        }

        // BLINK/WEBKIT
        &::-webkit-slider-thumb {
            appearance: none;
            width: 2rem;
            height: 2rem;
            border: none;
            border-radius: 1rem;
            box-shadow: 0 0 0 0.3em $bg;
            background: $fg;
            transform: scale(0.5);
            transition: transform 0.3s ease-out;
        }
        &::-webkit-slider-thumb:focus,
        &::-webkit-slider-thumb:active {
            appearance: none;
            transform: scale(0.85);
        }
    }
}

// test for Safari
@supports (-webkit-backdrop-filter: blur(2em)) {
    input[type='color'] {
        display: none;
    }
    input {
        border-radius: 1em;
        padding: 0.25em 0.5em;
    }
}

.colors {
    flex: 2;
    display: flex;
    flex-direction: column;
    font-size: 0.85em;

    .color {
        flex: 1;
        display: flex;
        align-items: center;
        padding: 0 1em;
        transition: 300ms linear;
    }
}
</style>
