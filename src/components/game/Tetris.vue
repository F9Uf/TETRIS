<template>
    <div class="wrapper">
        <div class="tetris">
            <display :text="`SCORE : ${score}`" />
            <stage :stage="stage"/>
            <div class="row">
                <base-button @click.native="startGame()" text="↻" />
                <base-button @click.native="move('up')" text="⯅" />
                <base-button @click.native="move('down')" text="⯆" />
                <base-button @click.native="move('left')" text="⯇" />
                <base-button @click.native="move('right')" text="⯈" />
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
            // reset everthing
            this.stage = this.createArray()
            this.resetPlayer()
            console.log('re-render')
        },
        drop () {
            this.updatePlayerPos({ x: 0, y: 1, collided: false })
        },
        dropplayer () {
            this.drop()
        },
        moveplayer (dir) {
            this.updatePlayerPos({ x: dir, y: 0})
        },
        move (direction) {
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
        },
        // player
        updatePlayerPos ({ x, y, collided }) {
            this.player = {
                ...this.player,
                pos: {
                    x: this.player.pos.x += x,
                    y: this.player.pos.y += y
                },
                collided
            }
        },
        resetPlayer () {
            this.player = {
                pos: {
                    x: STAGE_WIDTH / 2 - 2,
                    y: 0
                }, 
                tetromino: randomTetromino().shape,
                collided: false
            }
        },
        // stage
        updateStage () {
            // first flush the stage
            const prevStage = [ ...this.stage ]
            const newStage = prevStage.map(row =>
                row.map(cell => (cell[1] === 'clear' ? [0, 'clear'] : cell))
            )

            // then draw the tetromino
            this.player.tetromino.forEach((row, y) => {
                row.forEach((value, x) => {
                    if (value !== 0) {
                        newStage[y + this.player.pos.y][x + this.player.pos.x] = [
                            value,
                            `${this.player.collided ? 'merged' : 'clear'}`
                        ]
                    }
                })
            })

            this.stage = newStage
        }
    },
    created () {
        this.startGame()
    },
    watch: {
        player (after, before) {
            this.updateStage()
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