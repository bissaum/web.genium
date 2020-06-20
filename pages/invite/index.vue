<template lang="pug">
Card.invite
  p(slot="title") Convidar

  Form(ref="validateInvite" :model="formInvite" :rules="ruleInvite" label-position="top")

    FormItem(label="E-mail" prop="email")
      Input(v-model="formInvite.email" type="email" size="large" placeholder="Informe o e-mail")

    Row(type="flex" justify="end")
      Button(@click="submitInvite()" type="primary" size="large") Enviar convite
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import iView from 'iview'
Vue.use(iView)

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

  public submitInvite() {
    const form = this.$refs.validateInvite as any

    form.validate((valid: Boolean) => {
      if (valid) {
        this.$Message.success('Success!')
      } else {
        this.$Message.error('Fail!')
      }
    })
  }
}
</script>

<style lang="sass" scoped></style>
