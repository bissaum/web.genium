<template lang="pug">
Card
  p(slot="title") Enviar convite

  Form(ref="validateInvite" :model="formInvite" :rules="ruleInvite" label-position="left" :label-width="70")

    FormItem(label="E-mail" prop="email")
      Input(v-model="formInvite.email" type="email" size="large" placeholder="Informe o e-mail")

    Row(type="flex" justify="space-between")
      Button(@click="$router.push('/auth')" type="text" size="large") Cancelar
      Button(@click="submitInvite()" type="primary" size="large") Enviar
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

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
export default class Invite extends Vue {
  public formInvite: typeInvite = {
    email: ''
  }

  public ruleInvite: typeInvite = {
    email: [
      { required: true, message: 'Nenhum e-mail informado', trigger: 'blur' },
      { type: 'email', message: 'E-mail inválido', trigger: 'blur' }
    ]
  }

  public submitInvite() {
    const form = this.$refs.validateInvite as any

    form.validate((valid: Boolean) => {
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
