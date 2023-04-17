<template>
    <button class='button' @click="this.onclick">
        <slot />
    </button>
</template>

<style>
.button {
    border-radius: 4px;
    font-weight: 600;
}

.button:hover {
    background-color: #649de7;
    cursor: pointer;
}
</style>

<script>
export default {
    props: {
        onclick: Function
    }
}
</script>