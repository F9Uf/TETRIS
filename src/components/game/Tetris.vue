<template>
    <div class="wrapper">
        <div class="tetris">
            <display text="Score" />
            <stage :stage="createArray()"/>
            <div class="row">
                <base-button text="up" />
                <base-button text="down" />
                <base-button text="left" />
                <base-button text="right" />
            </div>
        </div>
    </div>
</template>

<script>
import { createStage } from '@/helper/gameHelper'

import Stage from './Stage'
import Display from './Display'
import BaseButton from './BaseButton'

export default {
    components: {
        Display,
        Stage,
        BaseButton
    },
    methods: {
        createArray () {
            return createStage()
        }
    }
}
</script>

<style lang="scss">

.wrapper {
    width: 100vw;
    height: 100vh;
    background-color: plum;
    overflow: hidden;
}

.tetris {
    display: flex;
    align-items: flex-start;
    padding: 40px;
    margin: 0 auto;
    max-width: 900px;
    flex-direction: column;
}
</style>