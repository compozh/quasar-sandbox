<template lang="pug">
  q-page.flex.flex-center
    .q-pa-md

      q-form(@submit="onSubmit"
             @reset="onReset")

        the-date-picker-input(v-model="date")

        q-list(separator dense bordered padding class="rounded-borders")
          q-item-label(header) Accounts

          q-scroll-area(:thumb-style="thumbStyle"
                        :bar-style="barStyle"
                        style="height: 176px")

            q-item(tag="label" dense
                   v-ripple :key="index"
                   v-for="(account, index) in accounts")

              q-item-section(side top)
                q-checkbox(v-model="checkedAccounts[account.name]")
              q-item-section
                q-item-label(class="ellipsis") {{ account.name }}
              q-item-section(v-if="checkedAccounts[account.name]")
                q-item-label sum
</template>

<script>
import TheDatePickerInput from '../components/TheDatePickerInput'

export default {
  components: {
    TheDatePickerInput
  },
  data: () => ({
    name: null,
    age: null,
    date: '',
    checkedAccounts: {},
    accept: false,
    thumbStyle: {
      right: '4px',
      borderRadius: '5px',
      backgroundColor: '#027be3',
      width: '5px',
      opacity: 0.75
    },
    barStyle: {
      right: '2px',
      borderRadius: '9px',
      backgroundColor: '#027be3',
      width: '9px',
      opacity: 0.2
    },
    accounts: [
      {
        name: 'Cash',
        balance: 1000.00,
        visible: true
      },
      {
        name: 'Credit',
        balance: -3000.00,
        visible: true
      },
      {
        name: 'UkrSib',
        balance: 5000.00,
        visible: true
      },
      {
        name: 'Privat',
        balance: 0.00,
        visible: true
      }
    ]
  }),
  methods: {
    onSubmit () {
      if (this.accept !== true) {
        this.$q.notify({
          color: 'red-5',
          textColor: 'white',
          icon: 'warning',
          message: 'You need to accept the license and terms first'
        })
      } else {
        this.$q.notify({
          color: 'green-4',
          textColor: 'white',
          icon: 'cloud_done',
          message: 'Submitted'
        })
      }
    },
    onReset () {
      this.name = null
      this.age = null
      this.accept = false
    }
  }
}
</script>
