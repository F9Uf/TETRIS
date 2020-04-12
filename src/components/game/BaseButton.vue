<template>
    <div :class="`btn ${type ? 'btn-' + type : ''}`">
        {{ text }}
    </div>
</template>

<script>
export default {
    props: ['text', 'type']
}
</script>

<style lang="scss" scoped>
$prim_btn1: blueviolet;
$sec_btn1: rgb(102, 2, 196);
$prim_btn2: #f59542; 
$sec_btn2: #f56c42;
$text_btn: white;

.btn {
    background-color: $prim_btn1;
    width: 40px;
    height: 40px;
    border-radius: 100%;
    color: $text_btn;
    outline: none;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    box-shadow: 0px 5px $sec_btn1;
    position: block;
    z-index: 1;

    &:hover {
        position: relative;
        box-shadow: 0px 3px $sec_btn1;
        top: 2px;
    }
    &:active {
        position: relative;
        box-shadow: 0px 0px;
        top: 5px;
        background-color: $sec_btn1;
    }

    &.active {
        position: relative;
        box-shadow: 0px 0px;
        top: 5px;
        background-color: $sec_btn1;
    }

    &.btn-2 {
        background-color: $prim_btn2;
        box-shadow: 0px 5px $sec_btn2;

        &:hover {
            position: relative;
            box-shadow: 0px 3px $sec_btn2;
            top: 2px;
        }
        &:active {
            position: relative;
            box-shadow: 0px 0px;
            top: 5px;
            background-color: $sec_btn2;
        }
        &.active {
        position: relative;
        box-shadow: 0px 0px;
        top: 5px;
        background-color: $sec_btn2;
    }
    }
}
</style>