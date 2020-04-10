<template>
    <div class="home">
        <Tetris />
    </div>
</template>

<script>
// @ is an alias to /src
import Tetris from '@/components/game/Tetris'

export default {
    name: 'Home',
    components: {
        Tetris
    }
}
</script>

<style lang="scss">
</style>
