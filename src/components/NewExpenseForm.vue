<template>
  <v-card>
    <v-card-title>New expense</v-card-title>
    <v-card-text>
      <v-form
        ref="form"
        v-model="valid"
      >
        <v-text-field
          label="Name"
          v-model="name"
          :rules="[rules.required, rules.max]"
        />
        <v-text-field
          label="Amount"
          v-model="amount"
          type="number"
          prefix="$"
          :rules="[rules.required, rules.max]"
        />
        <v-menu
          v-model="menu"
          :close-on-content-click="false"
          :nudge-right="40"
          transition="scale-transition"
          offset-y
          min-width="auto"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-text-field
              v-model="date"
              :rules="[rules.required, rules.max]"
              label="Date"
              prepend-icon="mdi-calendar"
              readonly
              v-bind="attrs"
              v-on="on"
            />
          </template>
          <v-date-picker
            v-model="date"
            @input="menu = false"
          />
        </v-menu>
      </v-form>
    </v-card-text>
    <v-card-title>
      <v-btn
        @click="onClick({ name, date, amount })"
        :disabled="!valid"
      >ADD</v-btn>
    </v-card-title>
  </v-card>
</template>

<script>
import moment from 'moment';

export default {
  name: 'NewExpenseForm',
  props: {
    dialog: Boolean
  },
  data() {
    return {
      name: '',
      date: moment().format('YYYY-MM-DD'),
      amount: '',
      menu: false,
      rules: {
        required: value => !!value || 'This field is required.',
        max: value => value.length <= 50 || 'Max 50 characters allowed.',
      },
      valid: false
    }
  },
  watch: {
    dialog(value) {
      if(value) {
        this.$refs.form.resetValidation();
      }
    }
  },
  methods: {
    onClick(data) {
      const { form } = this.$refs;
      if (form.validate()) {
        this.$emit('create', data);
        this.name = '';
        this.date = moment().format('YYYY-MM-DD');
        this.amount = 0;
        form.resetValidation();
      }
    }
  }
}
</script>
<style>
  input::-webkit-outer-spin-button,
  input::-webkit-inner-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
  input[type=number] {
    -moz-appearance: textfield;
  }
</style>
