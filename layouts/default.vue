
<!-- AQUI ESTA EL DIALOG DE CREACION DE TAREAS Y TAMBIEN LA NAVBAR -->


<template>
  <v-app dark>
    <v-navigation-drawer v-model="drawer" :mini-variant="miniVariant" :clipped="clipped" fixed app>
      <v-list>
        <v-list-item v-for="(item, i) in items" :key="i" :to="item.to" router exact>
          <v-list-item-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar :clipped-left="clipped" fixed app>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
      <v-btn icon @click.stop="miniVariant = !miniVariant">
        <v-icon>mdi-{{ `chevron-${miniVariant ? 'right' : 'left'}` }}</v-icon>
      </v-btn>
      <v-toolbar-title>{{ title }}</v-toolbar-title>
      <v-spacer />
      <v-row justify="end">
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
                <v-row>
                  <v-col cols="12" sm="6" md="4">
                    <v-text-field v-model="task.name" label="Task Name" required></v-text-field>
                  </v-col>
                  <v-col cols="12">
                    <v-textarea v-model="task.description" label="Description" required></v-textarea>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-date-picker v-model="task.dates" label="Dates" range required></v-date-picker>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-time-picker v-model="task.time" label="Time" required></v-time-picker>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-select v-model="task.priority" :items="['Low', 'Medium', 'High']" label="Priority"
                      required></v-select>
                  </v-col>
                  <v-col cols="12" sm="6" md="4">
                    <v-select v-model="task.status" :items="['Pending', 'Completed', 'Overdue']" label="Status"
                      required></v-select>
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="dialog = false">
                Close
              </v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="saveTask">
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: 'DefaultLayout',
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Home Page',
          to: '/',
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire',
        },
      ],
      dialog: false,
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'To do - App',
      task: {
        name: '',
        description: '',
        dates: null,
        time: null,
        priority: '',
        status: ''
      }
    }
  },
  methods: {
    saveTask() {
      const savedTask = { ...this.task };
      localStorage.setItem('savedTask', JSON.stringify(savedTask));
      // console.log(savedTask);
      this.dialog = false;
    }
  }
}
</script>

