<template lang="pug">
  q-layout(view="lHh Lpr lFf")
    q-header(elevated)
      q-toolbar
        q-btn(flat dense round
              @click="leftDrawerOpen = !leftDrawerOpen"
              icon="menu" aria-label="Menu")

        q-toolbar-title Quasar App

        lang-switcher

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
      router-view
</template>

<script>
import LangSwitcher from 'components/LangSwitcher'
import EssentialLink from 'components/EssentialLink'

export default {
  name: 'MainLayout',
  components: {
    LangSwitcher,
    EssentialLink
  },
  data: () => ({
    leftDrawerOpen: false,
    essentialLinks: [
      {
        title: 'Docs',
        caption: 'quasar.dev',
        icon: 'school',
        link: 'https://quasar.dev'
      }
    ]
  })
}
</script>
