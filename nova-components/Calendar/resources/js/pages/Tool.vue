<template>
  <FullCalendar ref="fullCalendar" :options="calendarOptions" />
</template>

<script>

import FullCalendar from '@fullcalendar/vue3'
import dayGridPlugin from '@fullcalendar/daygrid'
// import interactionPlugin from '@fullcalendar/interaction'

export default {
    components: {
    FullCalendar
    },
  data() {
    return {
      calendarOptions: {
        plugins: [ dayGridPlugin ],
        initialView: 'dayGridMonth',
        events: [],
        },
      users: []
    }
    },
   created() {
        this.getUsers();
    },
  mounted() {
    //
    },
  methods: {
    getUsers() {
    this.calendarOptions.events = []
      Nova.request().get('/api/users')
        .then(response => {
            this.users = response.data;
            this.users.forEach(user => {
                this.calendarOptions.events = [{
                    title: user.name,
                    start: user.created_at,
                }];
            });
        })
        .catch(error => {
          console.error(error);
        });
    },
  },
}
</script>

<style>
/* Scoped Styles */
</style>
