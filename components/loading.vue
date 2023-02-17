<template>
    <div class="loading">
        <span></span>
    </div>
</template>

<script>
export default {
    setup () {
        

        return {}
    }
}
</script>

<style lang="scss" scoped>

.loading{
    height: 100vh;
    width: 100%;
    display:  flex;
    justify-content: center;
    align-items: center;

    @keyframes cirrcle-loading {
        to{
            transform: rotateZ(360deg);
        }
    }

    span{
        display: block;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        border: 2px solid transparent;
        border-top: #fff;
        animation: circle-loading 1200ms ease infinite;
    }
}

</style>