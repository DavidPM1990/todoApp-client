<template>
    <div>
        <div v-if="cardVisible">
            <v-card class="mx-auto" max-width="300" max-high="200">
                <v-card-text>
                    <p class="text-h4 text--primary">Name: {{ taskData[0]?.name }}</p>
                    <div class="text--primary">
                        {{ taskData[0]?.description }}
                    </div>
                    <p>Start date: {{ taskData[0]?.start_date }}</p>
                    <p>End date: {{ taskData[0]?.end_date }}</p>
                    <p class="mt-5">Priority: {{ taskData[0]?.priority }}</p>
                </v-card-text>
                <v-card-actions class="custom-card-actions">
                    <div class="custom-checkbox-container">
                        <CheckBox @checkbox-changed="completeTask" />
                    </div>
                </v-card-actions>
            </v-card>
        </div>
    </div>
</template>

<script>

import CheckBox from '../components/CheckBox.vue';
import axios from '@/static/axios.js'

export default {
    data() {
        return {
            taskData: [],
            cardVisible: true,
        }
    },
    components: {
        CheckBox
    },
    mounted() {
        this.$nextTick(async () => {
            await Promise.all([this.GET_DATA('tasks')]).then((val) => {
                this.taskData = val[0]
                console.log('xxx', this.taskData)
            })
        })
    },
    methods: {
        completeTask(checked) {
            this.cardVisible = !checked;
        },
        async GET_DATA(path) {
            const response = await axios.get(path)
            return response.data
        }
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