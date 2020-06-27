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

    FormItem(label="E-mail")
      Row(type="flex" justify="space-between")
        b(v-text="email")
        a(@click="anotherEmail") Outro e-mail

    FormItem(label="Senha" prop="password")
      Input(
        password
        placeholder="Informe a senha"
        ref="password"
        size="large"
        type="password"
        v-model="model.password"
      )

    FormItem
      Row(type="flex" justify="space-between")
        Button(
          @click="$router.push('/auth/forgot')"
          size="large"
        ) Esqueci a senha
        Button(
          @click="submitSignIn"
          type="primary"
          size="large"
        ) Entrar
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

interface typeSignIn {
  password: string
}

interface typeRule {
  password: Array<optionsRule>
}

interface optionsRule {
  required?: boolean
  type?: string
  min?: number
  max?: number
  message: string
  trigger: string
}

@Component({
  layout: 'auth'
})
export default class SignIn extends Vue {
  public email: string = process.browser ? localStorage.email : ''

  public model: typeSignIn = {
    password: ''
  }

  public rules: typeRule = {
    password: [
      {
        required: true,
        message: 'Nenhuma senha informada',
        trigger: 'blur'
      },
      {
        type: 'string',
        min: 6,
        message: 'Senha mínima de 6 caracteres',
        trigger: 'blur'
      },
      {
        type: 'string',
        max: 8,
        message: 'Senha máxima de 8 caracteres',
        trigger: 'blur'
      }
    ]
  }

  mounted() {
    const password = this.$refs.password as HTMLElement
    password.focus()
  }

  public anotherEmail() {
    localStorage.rememberEmail = false
    this.$router.push('/auth')
  }

  public submitSignIn() {
    const form = this.$refs.form as HTMLFormElement

    form.validate((valid: boolean) => {
      if (valid) {
        this.$router.push('/home')
      }
    })
  }
}
</script>

<style lang="sass" scoped></style>
