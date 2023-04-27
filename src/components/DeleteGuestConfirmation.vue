<template>
    <div class="buttonContainer">
        <Button :onclick="onConfirm">Yes</Button>
        <Button :onclick="onCancel">No</Button>
    </div>
</template>

<style scoped>
.buttonContainer {
    display: flex;
    width: 30%;
    text-align: center;
    margin: 0px auto 32px auto;
    justify-content: space-between;
}

Button {
    padding: 8px 24px;
}
</style>

<script>
import Button from './Button.vue';

export default {
    components: {
        Button
    },

    props: {
        onConfirm: Function,
        onCancel: Function
    }
}
</script>