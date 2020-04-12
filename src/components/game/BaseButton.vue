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
$prim_btn: blueviolet;
$sec_btn: rgb(102, 2, 196);
$text_btn: white;

.btn {
    background-color: $prim_btn;
    width: 40px;
    height: 40px;
    border-radius: 100%;
    color: $text_btn;
    outline: none;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    box-shadow: 0px 5px $sec_btn;
    position: block;
    z-index: 1;

    &:hover {
        position: relative;
        box-shadow: 0px 3px $sec_btn;
        top: 2px;
    }
    &:active {
        position: relative;
        box-shadow: 0px 0px;
        top: 5px;
        background-color: $sec_btn;
    }
}
</style>