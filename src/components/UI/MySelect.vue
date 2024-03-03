<template>
    <select 
        :value="modelValue" 
        @input="modelValue = $event.target.value"
        @change="handleChangeOption"
        class="select"
    >
        <option 
            disabled 
            value=""
        >Выберите рабочий проект
        </option>
        <option 
            v-for="project in projects" 
            :key="project.value" 
            :value="project.id"
            >{{project.name}}
        </option>
    </select>
</template>

<script>
export default {
    name: 'my-select',

    props: {
        modelValue: {
            type: String
        },
        projects: {
            type: Array,
            default: () => []
        }
    },
    methods: {
        handleChangeOption(event) {
            this.$emit('update:modelValue', event.target.value)
        }
    }

}
</script>

<style scoped>
    .select {
        display: block;
        padding: 5px 10px 5px 0;
        cursor: pointer;
        border: 1px solid black;
        border-radius: 5px;
    }
</style>