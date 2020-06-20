<template lang="pug">
Card.invite
  p(slot="title") Convidar
  Form(:model="formInvite" :rules="ruleInvite" label-position="top")

    FormItem(label="E-mail")
      Input(v-model="formInvite.email" size="large" placeholder="Informe o e-mail")

    Row(type="flex" justify="end")
      Button(type="primary" size="large") Enviar
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

interface typeInvite {
  email: String | Array<typeRule>
}

interface typeRule {
  required?: Boolean
  type?: String
  message: String
  trigger: String
}

@Component({
  layout: 'auth'
})
export default class SignIn extends Vue {
  public formInvite: typeInvite = {
    email: ''
  }

  public ruleInvite: typeInvite = {
    email: [
      { required: true, message: 'Informe um e-mail', trigger: 'blur' },
      { type: 'email', message: 'E-mail é inválido', trigger: 'blur' }
    ]
  }
}
</script>

<style lang="sass" scoped></style>
