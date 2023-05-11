<template>
  <div>

    <div class="mt-5">
      <CalendarTodo />
    </div>

    <div class="mt-5">
      <CardInfo />
    </div>

  </div>
</template>
  
<script>

import CalendarTodo from '../components/CalendarTodo.vue';
import CardInfo from '../components/CardInfo.vue';

export default {
  components: {
    CalendarTodo,
    CardInfo
  },
}

</script>