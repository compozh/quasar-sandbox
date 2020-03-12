<template lang="pug">
  q-select(v-model="lang"
    :options="langOptions"
    :label="$t('language')"
    dense
    borderless
    emit-value
    map-options
    options-dense
    style="min-width: 150px")
</template>

<script>
import languages from 'quasar/lang/index.json'
import { format } from 'quasar'

const appLanguages = languages.filter(lang =>
  [ 'ru', 'uk', 'en-us' ].includes(lang.isoName))

export default {
  name: 'LangSwitcher',
  data: () => ({
    langOptions: []
  }),
  computed: {
    lang: {
      get () {
        return this.$i18n.locale
      },
      set (lang) {
        this.$i18n.locale = lang
      }
    }
  },
  watch: {
    lang (lang) {
      import(/* webpackInclude: /(ru|uk|en-us)\.js$/ */
      'quasar/lang/' + lang)
        .then(lang => this.$q.lang.set(lang.default))
    }
  },
  created () {
    const { capitalize } = format
    const label = name => name.slice(0, 2).toUpperCase()
    this.langOptions = appLanguages.map(lang => ({
      label: capitalize(lang.nativeName),
      icon: label(lang.isoName),
      value: lang.isoName
    }))
  }
}
</script>

<style scoped>

</style>
