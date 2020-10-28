<template>
<div>
    <span class="group">
        <label class="container">
            <input id="male" name="sex" type="radio" value="муж" v-bind:checked="checked" @change="$emit('change', $event.target.value)" /><span class="checkmark"></span></label>

        <label for="male" class="title">Мужчина</label></span>

    <span class="group">
        <label class="container">
            <input id="female" name="sex" type="radio" value="жен" v-bind:checked="checked" @change="$emit('change', $event.target.value)" /><span class="checkmark"></span></label>
        <label for="female" class="title">Женщина</label>
    </span>
</div>
</template>

<script>
export default {
    name: 'CustomRadio',
    model: {
        prop: 'checked',
        event: 'change',
    },
    props: {
        checked: String,
    },
};
</script>

<style lang="scss">
div {
    width: 100%;
    display: flex;
    justify-items: flex-start;

    .group {
        width: 50%;
        display: flex;
        justify-content: flex-start;
        align-items: center;

        label.title {
            padding-left: 10px;
        }

        .container {
            position: relative;
            display: block;
            box-sizing: border-box;
            width: 20px;
            height: 20px;
            cursor: pointer;
            user-select: none;

            input {
                position: absolute;
                width: 0;
                height: 0;
                opacity: 0;
                cursor: pointer;

                &:checked+.checkmark {
                    border: 1px solid #4e4376;
                }

                &:checked+.checkmark::after {
                    display: block;
                    position: absolute;
                    width: 12px;
                    height: 12px;
                    border: none;
                    border-radius: 50%;
                    top: 3px;
                    left: 3px;
                    right: 3px;
                    bottom: 3px;
                    background: linear-gradient(#2b5876, #4e4376);
                }
            }

            .checkmark {
                position: absolute;
                box-sizing: border-box;
                top: 0;
                left: 0;
                border: 1px solid rgba(31, 32, 65, 0.25);
                padding-right: 0;
                width: 20px;
                height: 20px;
                border-radius: 50%;
            }

            .checkmark::after {
                content: '';
                display: none;
            }
        }
    }
}

@import '../assets/scss/mobile.scss';
</style>
