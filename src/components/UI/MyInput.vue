<template>
    <div>
        <input 
        v-model="value"
        @input="recordValue"
        type="text">
    </div>
</template>

<script>
    export default {
        name: 'my-input',
        
        
        data() {
            return {
                value: ""
            }
        },

        methods: {
           recordValue() {
            this.$emit('record', this.value)
           } 
        }
       
    } 
</script>

<style>


</style>