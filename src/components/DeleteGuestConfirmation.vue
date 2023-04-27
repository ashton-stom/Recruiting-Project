<template>
    <div class="container">
        <div class="buttonContainer">
            <Button :onclick="onConfirm">{{ isLoading ? 'Loading...' : 'Yes' }}</Button>
            <Button v-if="!this.isLoading" :onclick="onCancel">No</Button>
        </div>
    </div>
</template>

<style scoped>
.container {
    width: 100%;
    background-color: #eeeeee;
    display: flex;
    justify-content: center;
}

.buttonContainer {
    display: flex;
    width: 30%;
    text-align: center;
    margin: 0px auto 24px auto;
    justify-content: space-evenly;
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
        onCancel: Function,
        isLoading: Boolean
    }
}
</script>