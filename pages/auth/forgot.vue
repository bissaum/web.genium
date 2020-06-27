<template lang="pug">
Card
  p(slot="title") Lembrar senha

  Form(
    :label-width="100"
    :model="model"
    :rules="rules"
    label-position="left"
    ref="form"
  )

    FormItem(label="E-mail" prop="email")
      Input(
        @keyup.enter.native="submitForgot"
        placeholder="Informe o e-mail"
        ref="email"
        size="large"
        type="email"
        v-model="model.email"
      )

    FormItem
      Row(type="flex" justify="space-between")
        Button(
          @click="$router.push('/auth/sign-in')"
          size="large"
        ) Cancelar

        Button(
          @click="submitForgot"
          type="primary"
          size="large"
        ) Enviar
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

interface typeForgot {
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
export default class Forgot extends Vue {
  public model: typeForgot = {
    email: process.browser ? localStorage.email : ''
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
    const email = this.$refs.email as HTMLElement
    email.focus()
  }

  public submitForgot() {
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
