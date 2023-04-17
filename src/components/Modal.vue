<template>
    <div class="modal">
        <div class="modalContent">
            <h4 class="closeX" @click="this.onClose">X</h4>
            <h3 class="title">{{ title }}</h3>
            <slot />
        </div>
    </div>
</template>

<style scoped>
.modal {
    position: fixed;
    border-radius: 4px;
    z-index: 1;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, .5);
    backdrop-filter: blur(2px);
}

.modalContent {
    display: flex;
    flex-flow: row wrap;
    background-color: rgb(238, 238, 238);
    width: 50%;
    position: relative;
    top: 40%;
    transform: translateY(-50%);
    margin: 0 auto;
}

.title {
    width: 100%;
    color: black;
    padding: 0px;
    margin: 32px auto 32px auto;
}

.closeX {
    text-align: right;
    position: absolute;
    top: 4px;
    right: 4px;
    margin: 0px;
}
</style>

<script>

export default {
    props: {
        onClose: Function,
        title: String
    }
}
</script>