<template>
    <v-dialog v-model="dialog" persistent width="1024">
        <template v-slot:activator="{ on }">
            <v-btn color="blue-darken-1" v-on="on">
                Create Task
            </v-btn>
        </template>

        <v-card>
            <v-card-title>
                <span class="text-h5">Create your task</span>
            </v-card-title>
            <v-card-text>
                <v-container>
                    <v-form>
                        <v-row>
                            <v-col cols="12" sm="6" md="4">
                                <v-text-field v-model="task.name" label="Task Name" required></v-text-field>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col cols="12">
                                <v-textarea v-model="task.description" label="Description" required></v-textarea>
                            </v-col>
                        </v-row>
                        <v-row>
                            <v-col cols="12" sm="6" md="6">
                                <v-date-picker v-model="task.dates" label="Dates" range required></v-date-picker>
                            </v-col>
                            <v-col cols="12" sm="6" md="6">
                                <v-row class="mt-2">
                                    <v-col cols="12" class="mt-16 pt-16 d-flex justify-center">
                                        <v-select v-model="task.priority" :items="['Low', 'Medium', 'High']"
                                            label="Priority" required></v-select>
                                    </v-col>
                                    <v-col cols="12" class="d-flex justify-center">
                                        <v-select v-model="task.status" :items="['Pending', 'Overdue']" label="Status"
                                            required></v-select>
                                    </v-col>
                                </v-row>
                            </v-col>
                        </v-row>
                    </v-form>
                </v-container>
            </v-card-text>
            <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue-darken-1" variant="text" @click="dialog = false">
                    Close
                </v-btn>
                <v-btn color="blue-darken-1" variant="text" @click="saveAndClose">
                    Save
                </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>

import axios from '@/static/axios.js'


export default {
    data() {
        return {
            dialog: false,
            task: {
                name: "",
                description: "",
                dates: null,
                time: null,
                priority: "",
                status: ""
            }
        };
    },
    methods: {
        async createTask() {
            await axios.post('tasks', {
                name: this.task.name,
                description: this.task.description,
                start_date: this.task.dates[0],
                end_date: this.task.dates[1],
                priority: this.task.priority,
                status: 'outstanding'
            })
                .then(response => {
                    return response.data
                })
                .catch(err => {
                    console.log(err)
                });

        },
        saveAndClose() {
            this.createTask();
            this.closeDialog();
        },
        closeDialog() {
            this.dialog = false;
        }
    }
}
</script>
