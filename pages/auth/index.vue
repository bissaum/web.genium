<template lang="pug">
Card
  p(slot="title") Entrar

  Form(ref="validateIndex" :model="formIndex" :rules="ruleIndex" label-position="left" :label-width="70")

    FormItem(label="E-mail" prop="email")
      Input(v-model="formIndex.email" type="email" size="large" placeholder="Informe o e-mail")

    FormItem
      Row(type="flex" justify="space-between")
        Button(@click="submitIndex()" type="primary" size="large") Entrar
        Checkbox(v-model="rememberEmail" size="large") Lembrar e-mail

    Divider

    Row(type="flex" justify="space-around")
      Button.google(icon="logo-google" size="large" shape="circle")
      Button.facebook(icon="logo-facebook" size="large" shape="circle")
      Button.twitter(icon="logo-twitter" size="large" shape="circle")
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
  public rememberEmail: boolean = !!(process.browser && localStorage.email)

  public formIndex: typeIndex = {
    email: process.browser ? localStorage.email : ''
  }

  public ruleIndex: typeRule = {
    email: [
      { required: true, message: 'Nenhum e-mail informado', trigger: 'blur' },
      { type: 'email', message: 'E-mail inválido', trigger: 'blur' }
    ]
  }

  public submitIndex() {
    const form = this.$refs.validateIndex as any

    form.validate((valid: boolean) => {
      if (valid) {
        if (this.rememberEmail) {
          localStorage.email = this.formIndex.email
          this.$Message.success('Seu e-mail será lembrado')
        } else {
          localStorage.email = ''
        }

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
