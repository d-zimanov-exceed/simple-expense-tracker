<template>
  <div class="tracker_wrapper">
    <div>
      <h1>Welcome to basic expenses tracker!</h1>
    </div>
    <v-container>
      <v-card>
        <v-card-text>
            <Chart
              :chartData="chartData"
              :height="200"
              :options="options"
            />
          <div class="tracker_table_section">
            <div class="tracker_table_header">
              <h2>Recent expenses</h2>
              <v-select
                v-model="sort"
                :items="sortItems"
                class="px-5"
                label="Display"
              />
                <v-dialog
                  v-model="dialog"
                  max-width="400px"
                >
                  <template v-slot:activator="{on}">
                    <div class="tracker_button_section">
                      <v-btn
                        v-on="on"
                      >
                        Add new expense
                      </v-btn>
                    </div>
                  </template>
                  <NewExpenseForm
                    @create="onCreate"
                    :dialog="dialog"
                  />
                </v-dialog>
            </div>
            <v-data-table
              :headers="headers"
              :items="expenses"
              :items-per-page="5"
              class="elevation-1"
            />
          </div>
        </v-card-text>
      </v-card>
    </v-container>
  </div>
</template>

<script>
  import moment from 'moment';
  import Chart from './Chart';
  import NewExpenseForm from './NewExpenseForm';
  import constants from '../constants';

  export default {
    name: 'ExpenseTracker',
    components: {
      Chart,
      NewExpenseForm,
    },
    data() {
      return {
        headers: [
          { text: 'Name', sortable: false, value: 'name' },
          { text: 'Date', value: 'date'},
          { text: 'Amount, $', value: 'amount' },
        ],
        expenses: [],
        sort: 'day',
        sortItems: [
          {
            text: 'Day',
            value: 'day',
          },
          {
            text: 'Month',
            value: 'month',
          },
          {
            text: 'Year',
            value: 'year'
          }
        ],
        dialog: false,
        options: {
          scales: {
            yAxes: [{
              ticks: {
                beginAtZero: true
              }
            }]
          },
          tooltips: {
            position: 'average',
            intersect: false
          },

        }
      };
    },
    computed: {
      chartData() {
        const { expenses } = this;
        const filteredExpenses = expenses
          .sort((a,b) => moment(a.date).diff(moment(b.date)))
          .reduce((acc, cur) => {
            const key = moment(cur.date).format(constants[this.sort]);
            
            if (this.sort === 'month') {
              moment.monthsShort().forEach((label) => {
                if (!acc[label]) {
                  acc[label] = 0;
                }
                return acc;
              })
            }

            if (!acc[key]) {
              acc[key] = cur.amount;
            } else {
              acc[key] += cur.amount;
            }
            return acc;
          }, {});
        const data = {
          labels: Object.keys(filteredExpenses),
          datasets: [
            {
              label: 'Amount',
              data: Object.values(filteredExpenses)
            }
          ]
        }
        return data;
      },
      chartLabels() {
        return this.expenses.map(item => item);
      },
    },
    methods: {
      onCreate(expense) {
        this.expenses = [
          ...this.expenses,
          {
            ...expense,
            amount: +expense.amount,
            date: moment(expense.date).format('YYYY-MM-DD')
          }
        ];
        this.dialog = false;
      }
    }
  }
</script>
<style>
.tracker_wrapper {
  height: 100%;
  width: 100%;
  max-width: 800px;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
.tracker_table_header {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
}
.tracker_button_section {
  display: flex;
  justify-content: center;
}
</style>