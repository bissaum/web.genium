<template lang="pug">
Card
  p(slot="title") Entrar

  Form(ref="validateSignIn" :model="formSignIn" :rules="ruleSignIn" label-position="left" :label-width="70")

    FormItem(label="E-mail")
      Row(type="flex" justify="space-between")
        b(v-text="email")
        a(@click="anotherEmail") Outro e-mail

    FormItem(label="Senha" prop="password")
      Input(v-model="formSignIn.password" type="password" size="large" password placeholder="Informe a senha" autofocus)

    FormItem
      Row(type="flex" justify="space-between")
        Button(@click="submitSignIn" type="primary" size="large") Entrar
        Button(@click="$router.push('/auth/forgot')" type="text" size="large") Esqueci a senha
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

  public formSignIn: typeSignIn = {
    password: ''
  }

  public ruleSignIn: typeRule = {
    password: [
      { required: true, message: 'Nenhuma senha informada', trigger: 'blur' },
      { type: 'string', min: 6, message: 'Senha mínima de 6 caracteres', trigger: 'blur' },
      { type: 'string', max: 8, message: 'Senha máxima de 8 caracteres', trigger: 'blur' }
    ]
  }

  public anotherEmail() {
    localStorage.rememberEmail = false
    this.$router.push('/auth')
  }

  public submitSignIn() {
    const form = this.$refs.validateSignIn as any

    form.validate((valid: boolean) => {
      if (valid) {
        this.$router.push('/home')
      }
    })
  }
}
</script>

<style lang="sass" scoped></style>
