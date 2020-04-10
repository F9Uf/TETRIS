<template>
    <div class="btn">
        {{ text }}
    </div>
</template>

<script>
export default {
    props: ['text']
}
</script>

<style lang="scss" scoped>
.btn {
    // display: block;
    background-color: blueviolet;
    width: 40px;
    height: 40px;
    border-radius: 100%;
    color: white;
    outline: none;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    box-shadow: 0px 3px 0.5px 2px orchid;
    transition: .3s;

    &:hover, &:active {
        transform: translateY(3px);
        box-shadow: 0 0 0 0 transparent;
    }
}
</style>