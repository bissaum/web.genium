<template lang="pug">
Layout.auth

  Header.header
    Logo

  Content
    Row.row(type="flex" justify="center" align="middle")
      Col(:xs="24" :sm="20" :md="16" :lg="12" :xl="10")
        nuxt

  Footer.footer
    Row(type="flex" justify="center")
      p Genium &copy; 2004
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
import 'view-design/dist/styles/iview.css'

import Logo from '@/components/Logo.vue'

Vue.use(ViewUI)

@Component({
  layout: 'auth',
  components: {
    Logo
  }
})
export default class SignIn extends Vue {
  public title: string = 'Convite'
}
</script>

<style lang="sass">
@import '@/assets/styles/layout.sass'

.auth
  min-height: 100vh

  .header
    height: 10vh
    background: transparent

  .row
    min-height: 80vh
    padding: 0 15px

  .footer
    height: 10vh

    p
      text-align: center
</style>
