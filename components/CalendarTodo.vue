<template>
    <div>
        <div>
            <v-sheet tile height="54" class="d-flex">
                <v-btn icon class="ma-2" @click="$refs.calendar.prev()">
                    <v-icon>mdi-chevron-left</v-icon>
                </v-btn>
                <v-select v-model="type" :items="types" dense outlined hide-details class="ma-2" label="type"></v-select>
                <v-spacer></v-spacer>
                <v-btn icon class="ma-2" @click="$refs.calendar.next()">
                    <v-icon>mdi-chevron-right</v-icon>
                </v-btn>
            </v-sheet>
            <v-sheet height="600">
                <v-calendar ref="calendar" v-model="value" :weekdays="weekday" :type="type" :events="events"
                    :event-overlap-threshold="30" :event-color="getEventColor" @change="getEvents">
                </v-calendar>
            </v-sheet>
        </div>
    </div>
</template>

<script>
import axios from '@/static/axios.js'

export default {
    data: () => ({
        localStorageValues: [],
        type: 'month',
        types: ['month', 'week', 'day', '4day'],
        weekday: [1, 2, 3, 4, 5, 6, 0],
        value: '',
        events: [],
        colors: ['blue', 'green', 'red'],
    }),
    methods: {
        async GET_DATA(path) {
            const response = await axios.get(path)
            return response.data
        },
        getEvents() {
            const events = []

            this.$nextTick(async () => {
                await Promise.all([this.GET_DATA('tasks')]).then((val) => {
                    this.tasks = val[0]

                    for (let i = 0; i < this.tasks.length; i++) {
                        const entry = this.tasks[i];

                        const event = {
                            name: entry.name,
                            start: new Date(entry.start_date), // Asigna la fecha de inicio adecuada segun su start date
                            end: new Date(entry.end_date), // Asigna la fecha de fin adecuada segun su end date
                            color: this.colors[this.rnd(0, this.colors.length - 1)], // El color se tiene que ajustar segun su status
                            timed: false,
                        };

                        events.push(event);
                    }

                    this.events = events
                })
            })
        },
        getEventColor(event) {
            return event.color
        },
        rnd(a, b) {
            return Math.floor((b - a + 1) * Math.random()) + a
        },
    }
}
</script>

<style scoped>
.my-event {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    border-radius: 2px;
    background-color: #1867c0;
    color: #ffffff;
    border: 1px solid #1867c0;
    font-size: 12px;
    padding: 3px;
    cursor: pointer;
    margin-bottom: 1px;
    left: 4px;
    margin-right: 8px;
    position: relative;
}

.my-event.with-time {
    position: absolute;
    right: 4px;
    margin-right: 0px;
}
</style>