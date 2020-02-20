<template lang="pug">
  q-page.flex.flex-center
    .q-pa-md(style="max-width: 400px")

      q-form(@submit="onSubmit"
             @reset="onReset").q-gutter-md
        div {{ date }}

        q-input(filled v-model="date" :mask="$t('dateInputMask')" :rules="[dateRules]")
          template(v-slot:append)
            q-icon(name="event" class="cursor-pointer")
              q-popup-proxy(ref="qDateProxy" transition-show="scale" transition-hide="scale")
                q-date(v-model="date"
                       :mask="$t('dateOutputMask')"
                       @input="() => $refs.qDateProxy.hide()")

        q-input(filled v-model="name"
                label="Your name *"
                hint="Name and surname"
                lazy-rules
                :rules="[ val => val && val.length > 0 || 'Please type something']")

        q-input(filled type="number"
                v-model="age"
                label="Your age *"
                lazy-rules
                :rules=`[val => val !== null && val !== '' || 'Please type your age',
                         val => val > 0 && val < 100 || 'Please type a real age']`)

        q-toggle(v-model="accept" label="I accept the license and terms")

        div
          q-btn(:label="$q.lang.label.create" type="submit" color="primary")
          q-btn(label="Reset" type="reset" color="primary" flat class="q-ml-sm")
</template>

<script>
import { date } from 'quasar'

export default {
  data: () => ({
    name: null,
    age: null,
    currentDate: new Date(),
    events: [ '2020/02/20', '2020/02/01' ],
    options: [ '2020/02/18', '2020/02/20', '2020/02/01', '2020/02/28' ],
    accept: false
  }),
  computed: {
    date: {
      get () {
        return date.formatDate(this.currentDate, this.$t('dateOutputMask'))
      },
      set (newDate) {
        this.currentDate = date.extractDate(newDate, this.$t('dateOutputMask'))
      }
    },
    dateRules () {
      return val => this.$t('dateInputMask').test(val) || this.$t('dateInvalidMessage')
    }
  },
  methods: {
    eventsFn (date) {
      if (date === '08.02.2020' ||
        date === '18.02.2020' ||
        date === '28.02.2020' ||
        date === '04.02.2020' ||
        date === '22.02.2020') {
        return true
      }
      return false
    },
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
