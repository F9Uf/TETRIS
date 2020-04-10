<template>
    <div class="display">
        {{ text }}
    </div>
</template>

<script>
export default {
    props: ['gameOver', 'text']

}
</script>

<style lang="scss">
.display {
    // width: 100%;
    // background-color: paleturquoise;
    padding-bottom: 10px;
    margin: 0 auto;
    color: white;
    font-size: 1.3em;
    font-weight: 600;
}
</style>