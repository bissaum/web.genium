<template lang="pug">
Card
  p(slot="title") Enviar convite

  Form(
    :label-width="70"
    :model="model"
    :rules="rules"
    label-position="left"
    ref="form"
  )

    FormItem(label="E-mail" prop="email")
      Input(
        @keyup.enter.native="submitInvite"
        placeholder="Informe o e-mail"
        ref="email"
        size="large"
        type="email"
        v-model="model.email"
      )

    Row(type="flex" justify="space-between")
      Button(
        @click="$router.push('/auth')"
        size="large"
      ) Cancelar

      Button(
        @click="submitInvite()"
        type="primary"
        size="large"
      ) Enviar
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

interface typeInvite {
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
export default class Invite extends Vue {
  public model: typeInvite = {
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
    const email = this.$refs.email as HTMLElement
    email.focus()
  }

  public submitInvite() {
    const form = this.$refs.form as any

    form.validate((valid: boolean) => {
      if (valid) {
        this.$Message.success('Convite foi enviado')
      } else {
        this.$Message.error('Convite não enviado')
      }
    })
  }
}
</script>

<style lang="sass" scoped></style>
