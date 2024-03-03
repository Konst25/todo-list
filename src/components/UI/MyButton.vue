<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
export default {
    name: "my-button",
}
</script>

<style scoped>
.btn {
    border-radius: 5px;
    padding: 5px 10px;
    border: 1px solid black;
    cursor: pointer;
}
</style>