<template lang="pug">
Card
  p(slot="title") Renovar senha

  Form(ref="validateForgot" :model="formForgot" :rules="ruleForgot" label-position="left" :label-width="70")

    FormItem(label="E-mail" prop="email")
      Input(v-model="formForgot.email" type="email" size="large" placeholder="Informe o e-mail")

    Row(type="flex" justify="space-between")
      Button(@click="$router.push('/auth')" type="text" size="large") Cancelar
      Button(@click="submitForgot()" type="primary" size="large") Enviar
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

interface typeForgot {
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
export default class Forgot extends Vue {
  public formForgot: typeForgot = {
    email: ''
  }

  public ruleForgot: typeForgot = {
    email: [
      { required: true, message: 'Nenhum e-mail informado', trigger: 'blur' },
      { type: 'email', message: 'E-mail inválido', trigger: 'blur' }
    ]
  }

  public submitForgot() {
    const form = this.$refs.validateForgot as any

    form.validate((valid: Boolean) => {
      if (valid) {
        this.$Message.success('E-mail foi enviado')
      } else {
        this.$Message.error('E-mail não enviado')
      }
    })
  }
}
</script>

<style lang="sass" scoped></style>
