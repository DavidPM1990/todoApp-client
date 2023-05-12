<template>
    <v-row>
        <v-col v-for="task in taskData" :key="task.id" :cols="3">
            <v-card class="mx-auto" max-width="300" max-high="200">
                <v-card-text>
                    <p class="text-h5 text--primary">{{ task.name | uppercase }}</p>
                    <div class="text--primary">{{ task.description }}</div>
                    <p>Start date: {{ task.start_date }}</p>
                    <p>End date: {{ task.end_date }}</p>
                    <p class="mt-5">Priority: {{ task.priority }}</p>
                </v-card-text>
                <v-card-actions class="custom-card-actions">
                    <div class="custom-checkbox-container">
                        <CheckBox @checkbox-changed="completeTask(task.id)" />
                    </div>
                </v-card-actions>
            </v-card>
        </v-col>
    </v-row>
</template>

<script>
import CheckBox from '../components/CheckBox.vue';
import axios from '@/static/axios.js'

export default {
    data() {
        return {
            taskData: [],
        }
    },
    components: {
        CheckBox
    },
    mounted() {
        this.fetchTaskData();
    },
    methods: {
        async completeTask(id) {
            await axios.delete(`tasks/${id}`)
                .then(response => {
                    console.log(response);
                    this.fetchTaskData();
                })
                .catch(err => {
                    console.log(err);
                });
        },
        async fetchTaskData() {
            await this.GET_DATA('tasks')
                .then(data => {
                    this.taskData = data;
                })
                .catch(err => {
                    console.log(err);
                });
        },
        async GET_DATA(path) {
            const response = await axios.get(path);
            return response.data;
        },
    },
    addTask(task) {
        this.taskData.push(task);
    },
    filters: {
        uppercase(value) {
            if (!value) return '';
            return value.toString().toUpperCase();
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