<template lang="pug">
Card
  p(slot="title") Nova senha

  Form(
    :label-width="120"
    :model="model"
    :rules="rules"
    label-position="right"
    ref="form"
  )

    FormItem(label="Código" prop="activePassword")
      Input(
        @keyup.enter.native="submitPassword"
        placeholder="Informe o código de ativação"
        required
        ref="active"
        autofocus
        size="large"
        type="tel"
        v-model="model.activePassword"
      )

    FormItem.password-meter(:class="meterPassword.status")
      p(v-text="meterPassword.message")
      Progress(
        :percent="meterPassword.percent"
        :status="meterPassword.status"
        :stroke-width="5"
        hide-info
      )

    FormItem(label="Criar senha" prop="password")
      Input(
        @keyup.enter.native="submitPassword"
        @keyup.native="scorePassword"
        password
        placeholder="Crie a nova senha"
        size="large"
        type="password"
        v-model="model.password"
      )

    FormItem
      Row(type="flex" justify="space-between")
        Button(
          @click="submitPassword"
          size="large"
          type="primary"
        ) Pronto
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

import ViewUI from 'view-design'
Vue.use(ViewUI)

interface typePassword {
  activePassword: string
  password: string
}

interface typeRule {
  activePassword: Array<optionsRule>
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

interface typeMeterPassword {
  percent: number
  status: string
  message: string
}

@Component({
  layout: 'auth'
})
export default class Password extends Vue {
  public model: typePassword = {
    activePassword: '',
    password: ''
  }

  public rules: typeRule = {
    password: [
      {
        required: true,
        message: 'Nenhuma senha foi criada',
        trigger: 'blur'
      },
      {
        type: 'string',
        min: 5,
        message: 'Nova senha mínimo de 5 caracteres',
        trigger: 'blur'
      },
      {
        type: 'string',
        max: 10,
        message: 'Nova senha máximo 10 caracteres',
        trigger: 'blur'
      }
    ],

    activePassword: [
      {
        required: true,
        message: 'Nenhuma código de ativação foi informado',
        trigger: 'blur'
      }
    ]
  }

  public meterPassword: typeMeterPassword = {
    percent: 0,
    status: 'normal',
    message: 'Sem senha'
  }

  mounted() {
    const active = this.$refs.active as HTMLElement
    active.focus()
  }

  public scorePassword() {
    const password = this.model.password
    const meter = this.meterPassword

    meter.percent = 0

    if (password.length > 4) {
      meter.percent += 20
      meter.status = 'normal'
      meter.message = 'Senha curta'
    }

    if (/[a-z]/.test(password)) {
      meter.percent += 20
      meter.status = 'wrong'
      meter.message = 'Senha fraca, tente usar números'
    }

    if (/[0-9]/.test(password)) {
      meter.percent += 20
      meter.status = 'active'
      meter.message = 'Está melhorando, tente usar maiúsculas'
    }

    if (/[A-Z]/.test(password)) {
      meter.percent += 20
      meter.status = 'active'
      meter.message = 'Quase lá, use caracteres especiais'
    }

    if (/[$@$!%*#?&]/.test(password)) {
      meter.percent += 20
      meter.status = 'success'
      meter.message = 'Perfeito, sua senha é segura'
    }
  }

  public submitPassword() {
    const form = this.$refs.form as HTMLFormElement

    form.validate((valid: boolean) => {
      if (valid) {
        this.$router.push('/home')
      }
    })
  }
}
</script>

<style lang="sass" scoped>
@import '../../assets/styles/layout'

.password-meter
  margin: -12px 0 0 0
  height: 35px

  p
    margin: 0
    height: 12px

  &.normal p
    color: $content

  &.active p
    color: $info

  &.success p
    color: $success

  &.wrong p
    color: $error
</style>
