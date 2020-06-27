<template lang="pug">
Card
  p(slot="title") Entrar

  Form(
    :label-width="100"
    :model="model"
    :rules="rules"
    label-position="left"
    ref="form"
  )

    FormItem(label="E-mail" prop="email")
      Input(
        @keyup.enter.native="submitIndex"
        placeholder="Informe o e-mail"
        ref="email"
        size="large"
        type="email"
        v-model="model.email"
      )

    FormItem
      Row(type="flex" justify="space-between")
        Checkbox(
          v-model="rememberEmail"
          size="large"
        ) Lembrar e-mail

        Button(
          @click="submitIndex"
          type="primary"
          size="large"
        ) Entrar

    Divider

    Row(type="flex" justify="space-around")
      Button.google(
        icon="logo-google"
        size="large"
        shape="circle"
      )

      Button.facebook(
        icon="logo-facebook"
        size="large"
        shape="circle"
      )

      Button.twitter(
        icon="logo-twitter"
        size="large"
        shape="circle"
      )
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

interface typeIndex {
  email: string
}

interface typeRule {
  email: Array<optionsRule>
}

interface optionsRule {
  required?: boolean
  type?: string
  message: string
  trigger: string
}

@Component({
  layout: 'auth'
})
export default class Index extends Vue {
  public rememberEmail: boolean = false

  public model: typeIndex = {
    email: ''
  }

  public rules: typeRule = {
    email: [
      {
        required: true,
        message: 'Nenhum e-mail informado',
        trigger: 'blur'
      },
      {
        type: 'email',
        message: 'E-mail inválido',
        trigger: 'blur'
      }
    ]
  }

  mounted() {
    if (process.browser) {
      if (localStorage.rememberEmail === 'true') {
        this.$router.push('/auth/sign-in')
      }
    }

    const email = this.$refs.email as HTMLElement
    email.focus()
  }

  public submitIndex() {
    const form = this.$refs.form as HTMLFormElement

    form.validate((valid: boolean) => {
      if (valid) {
        localStorage.rememberEmail = this.rememberEmail
        localStorage.email = this.model.email
        this.$router.push('/auth/sign-in')
      }
    })
  }
}
</script>

<style lang="sass" scoped>
.google,
.facebook,
.twitter
  color: #fff
  border-color: #fff
  margin: 0 10px

.google
  background: #db4437

.facebook
  background: #4267b2

.twitter
  background: #1da1f2
</style>
