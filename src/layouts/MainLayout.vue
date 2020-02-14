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
                 label="Quasar Language"
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

      div  {{ $q.lang.getLocale() }}
      div  {{ $q.lang.isoName }}

      div  {{ $t('success') }}
      div  {{ $t('failed') }}

      router-view
</template>

<script>
import EssentialLink from 'components/EssentialLink'

export default {
  name: 'MainLayout',
  components: {
    EssentialLink
  },
  data () {
    return {
      lang: this.$i18n.locale,
      langOptions: [
        { value: 'en-us', label: 'English' },
        { value: 'ru-ru', label: 'Русский' },
        { value: 'uk-ua', label: 'Український' }
      ],
      leftDrawerOpen: false,
      essentialLinks: [
        {
          title: 'Docs',
          caption: 'quasar.dev',
          icon: 'school',
          link: 'https://quasar.dev'
        },
        {
          title: 'Github',
          caption: 'github.com/quasarframework',
          icon: 'code',
          link: 'https://github.com/quasarframework'
        },
        {
          title: 'Discord Chat Channel',
          caption: 'chat.quasar.dev',
          icon: 'chat',
          link: 'https://chat.quasar.dev'
        },
        {
          title: 'Forum',
          caption: 'forum.quasar.dev',
          icon: 'record_voice_over',
          link: 'https://forum.quasar.dev'
        },
        {
          title: 'Twitter',
          caption: '@quasarframework',
          icon: 'rss_feed',
          link: 'https://twitter.quasar.dev'
        },
        {
          title: 'Facebook',
          caption: '@QuasarFramework',
          icon: 'public',
          link: 'https://facebook.quasar.dev'
        }
      ]
    }
  },
  watch: {
    lang (lang) {
      this.$i18n.locale = lang
    }
  }
}
</script>
