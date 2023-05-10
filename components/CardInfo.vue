<template>
    <div>
        <div v-if="cardVisible">
            <v-card class="mx-auto" max-width="300" max-high="200">
                <v-card-text>
                    <p class="text-h4 text--primary">
                        {{ taskData ? taskData.name : 'Título' }}
                    </p>
                    <p>Time Start: {{ taskData ? taskData.time : 'Hora' }}</p>
                    <div class="text--primary">
                        {{ taskData ? taskData.description : 'descripción' }}
                    </div>
                    <p class="mt-5">Priority: {{ taskData ? taskData.priority : 'Priority' }}</p>
                    <p>Status: {{ taskData ? taskData.status : 'Status' }}</p>
                    <p>Start Date: {{ taskData ? taskData.dates[0] : 'Start Date' }}</p>
                    <p>End Date: {{ taskData ? taskData.dates[1] : 'End Date' }}</p>
                </v-card-text>
                <v-card-actions class="custom-card-actions">
                    <div class="custom-checkbox-container">
                        <CheckBox @checkbox-changed="updateCardVisibility" />
                    </div>
                </v-card-actions>
            </v-card>
        </div>
    </div>
</template>

<script>

import CheckBox from '../components/CheckBox.vue';

export default {
    data() {
        return {
            taskData: null,
            cardVisible: true,
        }
    },
    components: {
        CheckBox
    },
    mounted() {
        const savedTask = localStorage.getItem('savedTask');
        if (savedTask) {
            this.taskData = JSON.parse(savedTask);
        }
    },
    methods: {
        updateCardVisibility(checked) {
            this.cardVisible = !checked;
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