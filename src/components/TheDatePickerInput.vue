<template lang="pug">
  q-input(outlined dense
          v-model="date"
          :rules="dateRules"
          :mask="$t('dateInputMask')")
    template(v-slot:append)
      q-icon(name="event" class="cursor-pointer")
        q-popup-proxy(ref="qDateProxy" transition-show="scale" transition-hide="scale")
          q-date(v-model="date" minimal
                 :mask="$t('dateOutputMask')"
                 :options="dateOptions"
                 @input="() => $refs.qDateProxy.hide()")
</template>

<script>
import { date } from 'quasar'

export default {
  name: 'TheDatePickerInput',
  data: () => ({
    currentDate: new Date(),
    stringDate: date.formatDate(new Date())
  }),
  computed: {
    date: {
      get () {
        return date.formatDate(this.currentDate, this.$t('dateOutputMask'))
      },
      set (newDate) {
        this.stringDate = newDate
        this.currentDate = date.extractDate(newDate, this.$t('dateOutputMask'))
        this.$emit('input', newDate)
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
  }
}
</script>
