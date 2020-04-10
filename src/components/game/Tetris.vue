<template>
    <div class="wrapper">
        <div class="tetris">
            <display :text="`SCORE : ${score}`" />
            <stage :stage="stage"/>
            <div class="row">
                <base-button @click.native="onChangeDirection('up')" text="⯅" />
                <base-button @click.native="onChangeDirection('down')" text="⯆" />
                <base-button @click.native="onChangeDirection('left')" text="⯇" />
                <base-button @click.native="onChangeDirection('right')" text="⯈" />
            </div>
        </div>
    </div>
</template>

<script>
import { createStage, STAGE_WIDTH } from '@/helper/gameHelper'
import { TETROMINOS, randomTetromino } from '@/helper/tetrominos'

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
            stage: [],
            score: 0,
            level: 1,
            rows: 0,
            player: {
                pos: { x:0, y: 0},
                tetromino: TETROMINOS[0].shape,
                collided: false
            }
        }
    },
    methods: {
        createArray () {
            return createStage()
        },
        startGame () {
            this.stage = createStage()
            this.dropTime = 1000
            this.player.tetromino = randomTetromino()
            this.player.x = parseInt(STAGE_WIDTH/2)
            this.player.y = 0
        },
        drop () {
            if (rows > (level + 1) * 10) {
                this.level = this.level + 1
                dropTime = (1000 / (level + 1) + 200)
            }
        },
        onChangeDirection (direction) {
            if (direction === 'up') {
                console.log('up')
            } else if (direction === 'down') {
                console.log('down')
            } else if (direction === 'left') {
                console.log('left')
            } else if (direction === 'right') {
                console.log('right')
            } else {
                console.log('hsldkfjlsdkjflkj')
            }
        }
    },
    created () {
        this.stage = createStage()
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