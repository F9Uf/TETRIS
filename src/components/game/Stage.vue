<template>
    <div
        class="stage"
        :style="`
            grid-template-rows: repeat(${stage.length}, ${cellWidth});
            grid-template-columns: repeat(${stage[0].length}, ${cellWidth});
        `"
    >
        <template v-for="(row, rowIndex ) in stage">
            <template v-for="(cell, cellIndex) in row">
                <Cell
                    :key="calIndex(rowIndex, cellIndex, row.length)"
                    :type="cell[0]"/>
            </template>
        </template>
    </div>
</template>

<script>
import Cell from './BaseCell'

export default {
    components: {
        Cell
    },
    props: {
        stage: Array
    },
    created () {
        window.addEventListener('resize', this.width)
    },
    data () {
        return {
            cellWidth: '10px'
        }
    },
    methods: {
        calIndex (rowIndex, colIndex, colSize) {
            return (rowIndex * colSize) + colIndex
        },
        width (e) {
            let windowWidth = window.innerWidth
            let windowHeight = window.innerHeight
            let row = this.stage.length
            let col = this.stage[0].length

            if (windowWidth > windowHeight) {
                // if width more than height, cal from height
                this.cellWidth = `calc(70vh / ${row})`
            } else {
                this.cellWidth = `calc(100vw / ${row})`
            }
        }
    },
    mounted () {
        this.width()
    }
}
</script>

<style>
.stage {
    display: grid;
    border: 2px solid #333;
    grid-gap: 1px;
    width: auto;
    min-width: 250px;
    height: auto;
    background: #111;
    margin: 0 auto;
}
</style>