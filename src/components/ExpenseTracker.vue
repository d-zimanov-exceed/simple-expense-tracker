<template>
  <v-container>
    <v-data-table
      :headers="headers"
      :items="expenses"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </v-container>
</template>

<script>
  export default {
    name: 'ExpenseTracker',
    data() {
      return {
        headers: [
          { text: 'Name', sortable: false, value: 'name' },
          { text: 'Type', value: 'type' },
          { text: 'Spent, $', value: 'value' },
        ],
        expenses: [
          {
            name: '1st expense',
            type: 'Other',
            value: 100,
          },
          {
            name: '2nd expense',
            type: 'Food',
            value: 10,
          },
          {
            name: '3rd expense',
            type: 'Other',
            value: 41,
          },
        ],
      };
    },
  }
</script>
