<template>
    <div class="wrapper">
        <div class="tetris">
            <display :text="`SCORE : ${score}`" />
            <stage :stage="createArray()"/>
            <div class="row">
                <base-button text="⯅" />
                <base-button text="⯆" />
                <base-button text="⯇" />
                <base-button text="⯈" />
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
    data () {
        return {
            dropTime: null,
            gameOver: false,
            stage: '',
            score: 0
        }
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

.row {
    display: flex;
    width: auto;
    margin: 0 auto;
    padding-top: 20px;

    > * {
        margin: 0 15px;
    }
}
</style>