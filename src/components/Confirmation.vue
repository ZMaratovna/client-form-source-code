<template>
<div class="confirmation_container">
    <h1>Новый клиент успешно создан!</h1>
</div>
</template>

<script>
export default {
    name: 'Confirmation',
};
</script>

<style lang="scss">
.confirmation_container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100vw;
    height: 100vh;
    background-color: transparent;
    border-radius: 14px;
    color: #fff;
    background-image: url('../assets/img/rocket_launch.svg');
    background-color: linear-gradient(#2b5876, #4e4376);
    background-blend-mode: hue;
    background-repeat: no-repeat;
    background-size: contain;
    background-position: right bottom;

    h1 {
        font-family: 'Montserrat';
        color: #fff;
        width: 60%;
    }
}

@import '../assets/scss/mobile.scss';
</style>
