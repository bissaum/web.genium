<template lang="pug">
Card
  p(slot="title") Entrar

  Form(ref="validateSignIn" :model="formSignIn" :rules="ruleSignIn" label-position="left" :label-width="70")

    FormItem(label="E-mail" prop="email")
      Input(v-model="formSignIn.email" type="email" size="large" placeholder="Informe o e-mail")

    FormItem(label="Senha" prop="password")
      Input(v-model="formSignIn.password" type="password" password size="large" placeholder="Informe a senha")

    Row(type="flex" justify="space-between")
      Button(@click="$router.push('/auth/forgot')" type="text" size="large") Esqueci a senha
      Button(@click="submitSignIn()" type="primary" size="large") Entrar
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

interface typeSignIn {
  email: String | Array<typeRule>
  password: String | Array<typeRule>
}

interface typeRule {
  required?: Boolean
  type?: String
  min?: Number
  max?: Number
  message: String
  trigger: String
}

@Component({
  layout: 'auth'
})
export default class SignIn extends Vue {
  public formSignIn: typeSignIn = {
    email: '',
    password: ''
  }

  public ruleSignIn: typeSignIn = {
    email: [
      { required: true, message: 'Nenhum e-mail informado', trigger: 'blur' },
      { type: 'email', message: 'E-mail inválido', trigger: 'blur' }
    ],
    password: [
      { required: true, message: 'Nenhuma senha informada', trigger: 'blur' },
      { type: 'string', min: 6, message: 'Senha no mínimo 6 caracteres', trigger: 'blur' },
      { type: 'string', max: 8, message: 'Senha no máximo de 8 caracteres', trigger: 'blur' }
    ]
  }

  public submitSignIn() {
    const form = this.$refs.validateSignIn as any

    form.validate((valid: Boolean) => {
      if (valid) {
        this.$Message.success('Autenticado')
      } else {
        this.$Message.error('Não autenticado')
      }
    })
  }
}
</script>

<style lang="sass" scoped></style>
