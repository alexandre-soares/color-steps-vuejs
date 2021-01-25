<template>
    <div class="app">
        <div class="tools">
            <h1>
                Color Steps : choose 2 colors and the number of steps between
                them. <br />
                Click on the color to add it to the clipboard.
            </h1>
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
                <div class="tools__steps">
                    {{ visualSteps }} {{ stepsLabel }}
                </div>
            </div>
            <div class="tools__second-color">
                <input type="color" v-model="color2" />
                <input type="text" v-model="color2" />
            </div>
        </div>
        <div class="colors">
            <div
                class="colors__color"
                v-for="(color, index) in colors"
                :style="setStyles(color)"
                :key="index"
                @click="copyColor(colorName(color))"
            >
                <input
                    type="hidden"
                    :id="colorName(color).substring(1)"
                    :value="colorName(color).substring(1)"
                />
                {{ colorName(color) }}
            </div>
        </div>

        <div class="logs">
            <transition-group name="list" tag="div" appear>
                <div class="log" v-for="(log, index) in logs" :key="index">
                    {{ log }}
                </div>
            </transition-group>
        </div>
    </div>
</template>

<script>
import functions from '../mixins/functions'
export default {
    data() {
        return {
            min: 3,
            max: 15,
            steps: 5,
            delta: 10,
            color1: '#FD6B42',
            color2: '#E342FD',
            logs: [],
        }
    },
    mixins: [functions],
    computed: {
        visualSteps() {
            return this.steps - 2
        },
        stepsLabel() {
            return this.visualSteps === 1 ? 'step' : 'steps'
        },
        colors() {
            return this.interpolateColors(this.color1, this.color2, this.steps)
        },
    },
    methods: {
        adjust(color) {
            const hex = this.rgbToHex(color[0], color[1], color[2])
            return this.foregroundAdjust(hex)
        },
        setStyles(color) {
            return `background: rgb(${color}); color: ${this.adjust(color)}`
        },
        colorName(color) {
            return this.rgbArrayToHex(color)
        },
        copyColor(color) {
            let testingCodeToCopy = document.getElementById(
                `${color.substring(1)}`
            )

            testingCodeToCopy.setAttribute('type', 'text')
            testingCodeToCopy.select()

            try {
                var successful = document.execCommand('copy')

                if (successful) {
                    this.logs.push(`The color ${color} has been copied!`)
                }
            } catch (err) {
                alert('Oops, unable to copy')
            }

            /* unselect the range */
            testingCodeToCopy.setAttribute('type', 'hidden')
            window.getSelection().removeAllRanges()
        },
    },
    mounted() {
        if (this.logs) {
            setInterval(() => {
                this.logs.shift()
            }, 5000)
        }
    },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$fg: #fff;
$bg: #2c3e50;

.app {
    height: 100vh;
    width: 100vw;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    position: relative;
}

.tools {
    padding: 4rem 2rem;
    font-size: 4rem;
    width: 80vw;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex: 0 0 30%;

    @media only screen and (max-width: 900px) {
        flex-direction: column;
        padding: 0;
    }

    & h1 {
        font-size: 2.5rem;
        position: absolute;
        text-align: center;
        width: 90%;
        top: 2%;
        left: 50%;
        transform: translateX(-50%);

        @media only screen and (max-width: 900px) {
            display: none;
        }
    }

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

    @media only screen and (max-width: 900px) {
        margin: 3rem 0;
        padding: 0;
    }

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
    width: 100%;
    flex: 0 0 70%;
    display: flex;
    flex-direction: column;
    font-size: 0.85em;
    text-align: center;

    &__color {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        font-size: 4rem;
        margin: 0 auto;
        width: 100vw;
        transition: 300ms linear;
        cursor: pointer;
    }
}

.logs {
    position: absolute;
    bottom: 1%;
    right: 1%;
}

.log {
    padding: 1rem 2rem;
    font-size: 1.4rem;
    background-color: lightgray;
    border-radius: 0.5rem;
    margin: 1rem;
}

.list-enter-active,
.list-leave-active {
    transition: all 1s;
}
.list-enter, .list-leave-to /* .list-leave-active below version 2.1.8 */ {
    opacity: 0;
    transform: translateX(30px);
}
</style>
