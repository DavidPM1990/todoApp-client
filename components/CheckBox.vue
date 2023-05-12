<template>
    <v-container class="px-0" fluid>
        <v-checkbox v-model="checkbox" :label="`Completado`" @change="handleCheckboxChange"></v-checkbox>
    </v-container>
</template>

<script>
export default {
    data() {
        return {
            checkbox: false,
        }
    },
    methods: {
        handleCheckboxChange(checked) {
            this.$emit('checkbox-changed', checked);
        }
    }
}
</script>