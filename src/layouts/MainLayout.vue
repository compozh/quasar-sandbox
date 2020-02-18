<template lang="pug">
  q-layout(view="lHh Lpr lFf")
    q-header(elevated)
      q-toolbar
        q-btn(flat dense round
              @click="leftDrawerOpen = !leftDrawerOpen"
              icon="menu" aria-label="Menu")

        q-toolbar-title Quasar App

        q-select(v-model="lang"
                 :options="langOptions"
                 :label="$t('language')"
                 dense
                 borderless
                 emit-value
                 map-options
                 options-dense
                 style="min-width: 150px")

        div Quasar v {{ $q.version }}

    q-drawer(v-model="leftDrawerOpen"
             show-if-above bordered
             content-class="bg-grey-1")
      q-list
        q-item-label(header).text-grey-8 Essential Links
        essential-link(v-for="link in essentialLinks"
                      :key="link.title"
                      v-bind="link")
    q-page-container

      p Users locale (based on browser settings): {{ $q.lang.getLocale() }}
      p Quasars internal locale: {{ $q.lang.isoName }}

      p test phrase: {{ $t('success') }}
      p test phrase: {{ $t('failed') }}

      router-view
</template>

<script>
import languages from 'quasar/lang/index.json'
import EssentialLink from 'components/EssentialLink'
import { format } from 'quasar'

const appLanguages = languages.filter(lang =>
  [ 'ru', 'uk', 'en-us' ].includes(lang.isoName))

export default {
  name: 'MainLayout',
  components: {
    EssentialLink
  },
  data: () => ({
    langOptions: [],
    leftDrawerOpen: false,
    essentialLinks: [
      {
        title: 'Docs',
        caption: 'quasar.dev',
        icon: 'school',
        link: 'https://quasar.dev'
      }
    ]
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
