<template>
    <div class="icon" @dblclick="openFolder">
        <img class="icon-img" :src="data.icon">
        <p class="icon-name">{{data.name}}</p>
    </div>
</template>

<script>
    export default {
        props:   ['data'],
        methods: {
            openFolder: function () {
                this.active = false;
                this.$emit('openApp');
            }
        }
    }
</script>

<style scoped>
    /* ICONS */
    .icon {
        z-index: 200;
        display: block;
        width: 100px;
        text-align: center;
        height: 100px;
        padding: 15px 10px;
        color: #ffffff;
        line-height: 17px;
        border: 1px solid transparent;
        -webkit-user-drag: none;
        -khtml-user-drag: none;
        -moz-user-drag: none;
        -o-user-drag: none;
        user-drag: none;
        user-select: none;
    }
    .icon .icon-img {
        width: 40px;
        height: 40px;
    }
    .icon .icon-name {
        position: relative;
        top: 3px;
        text-shadow: 0 3px 5px #3a3a3a;
    }
    .icon .icon-img,
    .icon .icon-name {
        -webkit-user-drag: none;
        -khtml-user-drag: none;
        -moz-user-drag: none;
        -o-user-drag: none;
        user-drag: none;
        user-select: none;
    }

    .icon:hover,
    .icon:focus {
        color: #ffffff;
        border: 1px solid #6060cb42;
        background: #27279c42;
    }
</style>