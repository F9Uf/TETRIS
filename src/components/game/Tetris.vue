<template>
    <div
        class="wrapper"
        
        @keypress.down="move('down')"
        @keydown.left="move('left')"
        @keyup.right="move('right')"
    >
        <div class="tetris" @keyup="move('sdflkjlk')">
            <display v-if="!gameOver" :text="`SCORE : ${score}`" />
            <display v-if="gameOver" :text="`Game Over!! Your Score: ${score} 🚀`" />
            <stage :stage="stage"/>
            <div class="row">
                <base-button @click.native="move('left')" :type="1" text="◀" />
                <base-button @click.native="move('up')" :type="1" text="▲" />
                <base-button @click.native="move('down')" :type="1" text="▼" />
                <base-button @click.native="move('right')" :type="1" text="▶" />
            </div>
            <div class="row">
                <base-button @click.native="startGame()" :type="2" text="↻" />
            </div>
        </div>
    </div>
</template>

<script>
import { createStage, STAGE_WIDTH, checkCollision } from '@/helper/gameHelper'
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
            rowCleared: 0,
            rows: 0,
            player: {
                pos: { x:0, y: 0},
                tetromino: TETROMINOS[0].shape,
                collided: false
            },
            interval: null,
            linePoints: [40, 100, 300, 1000]
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
            this.gameOver = false
            this.rowCleared = 0
            this.score = 0
            this.dropTime = 1000
            this.row = 0
            this.level = 1
            if (this.interval) {
                clearInterval(this.interval)
            }
            this.interval = setInterval(() => {
                this.drop()
            }, this.dropTime)
            console.log('re-render')
        },
        drop () {
            // up level
            if (this.rows > (this.level + 1) * 10) {
                this.level += 1
                this.dropTime = 1000 / (this.level + 1) + 2000
            }

            if (!checkCollision(this.player, this.stage, { x: 0, y: 1 })) {
                this.updatePlayerPos({ x: 0, y: 1, collided: false })
            } else {
                // game over
                if (this.player.pos.y < 1) {
                    console.log('game over')
                    this.gameOver = true
                    this.dropTime = null
                }
                this.updatePlayerPos({ x: 0, y: 0, collided: true })
            }
        },
        dropplayer () {
            this.drop()
        },
        moveplayer (dir) {
            if (!checkCollision(this.player, this.stage, { x: dir, y: 0 })) {
                this.updatePlayerPos({ x: dir, y: 0})
            }
        },
        move (direction) {
            if (!this.gameOver) {
                if (direction === 'up') {
                    this.playerRotate(this.stage, 1)
                } else if (direction === 'down') {
                    this.dropplayer()
                } else if (direction === 'left') {
                    this.moveplayer(-1)
                } else if (direction === 'right') {
                    this.moveplayer(1)
                } else {
                    console.log('no key')
                }
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
        rotate(matrix, dir) {
            // rotate
            const rotatedTetro = matrix.map((_, index) => 
                matrix.map(col => col[index])
            )
            // reverse each row
            if (dir > 0) return rotatedTetro.map(row => row.reverse())
            return rotatedTetro.reverse()
        },
        playerRotate (stage, dir) {
            const clonedplayer = JSON.parse(JSON.stringify(this.player))
            clonedplayer.tetromino = this.rotate(clonedplayer.tetromino, dir)

            const pos = clonedplayer.pos.x
            let offset = 1
            while (checkCollision(clonedplayer, stage, { x: 0, y: 0 })) {
                clonedplayer.pos.x += offset
                offset =  -(offset + (offset > 0 ? 1 : -1))
                if (offset > clonedplayer.tetromino[0].length) {
                    this.rotate(clonedplayer.tetromino, -dir)
                    clonedplayer.pos.x = pos
                    return
                }
            }

            this.player = clonedplayer
        },
        // stage
        updateStage () {
            // first flush the stage
            const prevStage = [ ...this.stage ]
            const newStage = prevStage.map(row =>
                row.map(cell => (cell[1] === 'clear' ? [0, 'clear'] : cell))
            )

            if (!checkCollision(this.player, prevStage, { x: 0, y: 0 })) {
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
            }
            // then check if we collided    
            if (this.player.collided) {
                this.resetPlayer()
                this.stage = this.sweepRows(newStage)
                return
            }

            this.stage = newStage
        },
        sweepRows (newStage) {
            return newStage.reduce((ack, row) => {
                if (row.findIndex(cell => cell[0] === 0) === -1) {
                    this.rowCleared = this.rowCleared + 1
                    ack.unshift(new Array(newStage[0].length).fill([0, 'clear']))
                    return ack
                }
                ack.push(row)
                return ack
            }, [])
        }
    },
    created () {
        this.startGame()
    },
    watch: {
        player (after, before) {
            this.updateStage()
        },
        rowCleared (after, before) {
            if (after > 0) {
                this.score += this.linePoints[this.rowCleared - 1] * (this.level + 1)
                this.row += this.rowCleared
                this.rowCleared = 0
            }
        },
        gameOver (after, before) {
            if (this.gameOver) {
                clearInterval(this.interval)
                this.player = {
                    pos: { x: 0, y: 0},
                    tetromino: TETROMINOS[0].shape,
                    collided: true
                }
            }
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