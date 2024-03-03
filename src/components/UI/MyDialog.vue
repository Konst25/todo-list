<template>
    <div class="dialog" v-if="show" @click.stop="hideDialog">
        <form class="dialog__content" @click.stop>
            <slot></slot>
        </form>
    </div>
</template>

<script>
export default {
    name: 'my-dialog',

    props: {
        show: {
            type: Boolean,
            default: false,
        }
    },
    methods: {
        hideDialog () {
            this.$emit("update:show", false);
        }, 
        
    }
}
</script>

<style>
.dialog {
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background: rgba(0, 0, 0, 0.5);
    position: fixed;
    display: flex;
}
.dialog__content {
    display: flex;
    flex-direction: column;
    margin: auto;
    background: #fbf0e1;
    border-radius: 5px;
    padding: 0 10px 10px 10px;
}
</style>