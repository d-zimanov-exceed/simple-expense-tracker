<template>
  <v-app>
    <v-main>
      <ExpenseTracker/>
    </v-main>
  </v-app>
</template>

<script>
import ExpenseTracker from './components/ExpenseTracker';

export default {
  name: 'App',
  components: {
    ExpenseTracker,
  }
};
</script>
