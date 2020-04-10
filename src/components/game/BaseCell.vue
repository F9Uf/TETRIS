<template>
    <div :class="`cell ${color}`">
        
    </div>
</template>

<script>
import { TETROMINOS } from '@/helper/tetrominos'
export default {
    props: ['type'],
    computed: {
        color () {
            return TETROMINOS[this.type].color
        }
    }
}
</script>

<style lang="scss">
$white: white;
$red: #ff0000;
$pink: #ff32a3;
$yellow: #fbff00;
$blue: #008cff;
$orange: #ff7300;
$purple: #b700ff;
$green: #00ff00;

@mixin cell-body($color) {
    background: rgba($color, 0.8);
    @if $color == $white {
        border: 0px solid;
    }
    @else {
        border: 4px solid;
    }
    border-bottom-color: rgba($color, 0.1);
    border-right-color: rgba($color, 1);
    border-top-color: rgba($color, 1);
    border-left-color: rgba($color, 0.3);
}

.cell {
    width: auto;

    &.white {
        @include cell-body($white);
    }
    &.red {
        @include cell-body($red);
    }
    &.pink {
        @include cell-body($pink);
    }
    &.yellow {
        @include cell-body($yellow);
    }
    &.blue {
        @include cell-body($blue);
    }
    &.orange {
        @include cell-body($orange);
    }
    &.purple {
        @include cell-body($purple);
    }
    &.green {
        @include cell-body($green);
    }
}
</style>