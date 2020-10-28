<template>
<label class="slider-container" style="width:40px, height:20px">
    <input type="checkbox" name="unsubscribe" value="true" v-bind:checked="checked" v-on:change="$emit('change', $event.target.checked)" />
    <span class="round"></span>
</label>
</template>

<script>
export default {
    name: 'ToggleButton',
    model: {
        prop: 'checked',
        event: 'change',
    },
    props: {
        checked: Boolean,
    },
};
</script>

<style lang="scss">
.slider-container {
    position: relative;
    display: block;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    width: 40px;
    height: 20px;
}

.round {
    position: absolute;
    box-sizing: border-box;
    -webkit-box-sizing: border-box;
    display: block;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;

    width: 40px;
    height: 20px;
    border-radius: 10px;
    border: 1px solid rgba(31, 32, 65, 0.25);
}

.round::before {
    position: absolute;
    content: '';
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background: rgba(31, 32, 65, 0.25);
    top: 3px;
    left: 3px;
    transition: 0.4s;
}

input[type='checkbox'] {
    position: absolute;
    opacity: 0;
    width: 0px;
    height: 0px;
}

input[type='checkbox']:checked+.round {
    border: 1px solid #4e4376;
    background: #fff;
}

input[type='checkbox']:checked+.round::before {
    background: linear-gradient(#2b5876, #4e4376);
    transform: translateX(20px);
}
</style>
