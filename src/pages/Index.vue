<template lang="pug">
  q-page.flex.flex-center
    .q-pa-md(style="max-width: 400px")

      q-form(@submit="onSubmit"
             @reset="onReset").q-gutter-md
        div {{ date }}

        q-input(filled v-model="date" :mask="$t('dateInputMask')" :rules="dateRules" dense)
          template(v-slot:append)
            q-icon(name="event" class="cursor-pointer")
              q-popup-proxy(ref="qDateProxy" transition-show="scale" transition-hide="scale")
                q-date(v-model="date" minimal
                       :mask="$t('dateOutputMask')"
                       :options="dateOptions"
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
    stringDate: date.formatDate(new Date()),
    accept: false
  }),
  computed: {
    date: {
      get () {
        return date.formatDate(this.currentDate, this.$t('dateOutputMask'))
      },
      set (newDate) {
        this.stringDate = newDate
        this.currentDate = date.extractDate(newDate, this.$t('dateOutputMask'))
      }
    },
    parseDate () {
      return date => {
        let day = '', month = '', year = ''
        const mask = this.$t('dateOutputMask')
        mask.split('').forEach((char, index) => {
          switch (char) {
            case 'D': day += date[index]
              break
            case 'M': month += date[index]
              break
            case 'Y': year += date[index]
              break
          }
        })
        return { day, month, year }
      }
    },
    dateRules () {
      const parsedDate = this.parseDate(this.stringDate)
      let { day, month, year } = parsedDate
      console.log('parsedDate', parsedDate)
      const maxYear = new Date().getFullYear() + 1
      const minYear = new Date().getFullYear() - 1
      const daysInMonth = new Date(year, month, 0).getDate()
      return [
        val => (year <= maxYear && year > minYear) || this.$t('dateInvalidYear'),
        val => (month <= 12 && month > 0) || this.$t('dateInvalidMonth'),
        val => (day <= daysInMonth && daysInMonth > 0) || this.$t('dateInvalidDay')
      ]
    },
    dateOptions () {
      const startDate = date.subtractFromDate(new Date(), { year: 1 })
      const endDate = date.addToDate(new Date(), { year: 1 })
      const dateF = unformedDate => date.formatDate(unformedDate, 'YYYY/MM/DD')
      return date => date >= dateF(startDate) && date <= dateF(endDate)
    }
  },
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
