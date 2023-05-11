<template>
    <div>{{ tasks }}
    </div>
</template>

<script>
import axios from '@/static/axios.js'

export default {
    data() {
        return {
            tasks: []
        };
    },
    methods: {
        async GET_DATA(path) {
            const response = await axios.get(path)
            return response.data
        }
    },
    mounted() {
        this.$nextTick(async () => {
            await Promise.all([this.GET_DATA('tasks')]).then((val) => {
                this.tasks = val[0]
            })
        })
    }
}
</script>

<style>
.custom-card-actions {
    display: flex;
    justify-content: center;
    align-items: center;
}

.custom-checkbox-container {
    display: flex;
    justify-content: center;
    align-items: center;
}
</style>