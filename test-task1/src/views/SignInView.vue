<script setup lang="ts">
import Input from '@/components/Input.vue'
import { reactive } from 'vue'
import { z } from 'zod'

const initialErrors = {
  email: undefined,
  password: undefined,
}

const initialValues = {
  email: '',
  password: '',
}

const schema = z.object({
  email: z.string().email().min(1).max(64),
  password: z.string().regex(/^(?=(.*[A-Z]))(?=(.*\d))(?=(.*[!@#$%^&*(),.?":{}|<>])).{8,}$/, {
    message:
      'Password must be at least 8 characters long, with at least one uppercase letter, one number, and one special character.',
  }),
})

const state = reactive({
  errors: initialErrors,
  values: initialValues,
})

function onSubmit(e: Event) {
  state.errors = initialErrors

  const parseResult = schema.safeParse(state.values)

  if (!parseResult.success) {
    const errors: Record<string, string | undefined> = {}
    for (const item of parseResult.error.issues) {
      if (item.path.length > 0) {
        errors[item.path[0]] = item.message
      }
    }

    // @ts-ignore
    state.errors = errors

    const captchaResponse = (window as any).hcaptcha.getResponse()

    if (!captchaResponse) {
      alert('Пожалуйста, подтвердите, что вы не робот!')
      return
    }

    // Отправка данных формы с токеном капчи на сервер
    console.log('hCaptcha Response:', captchaResponse)

    return
  }

  console.log('Ура вы вошли')
}
</script>

<template>
  <main class="max-w-80 mx-auto flex flex-col gap-6">
    <div class="text-lg font-medium text-gray-900">Введите данные для входа:</div>
    <form @submit.prevent="onSubmit" class="flex flex-col gap-5">
      <Input
        v-model="state.values.email"
        label="Введите e-mail"
        placeholder="example@example.com"
        :error="state.errors?.email"
        type="email"
      />
      <Input
        v-model="state.values.password"
        label="Введите пароль"
        placeholder="123456"
        :error="state.errors?.password"
        type="password"
      />

      <button
        type="submit"
        class="text-gray-900 bg-white border border-gray-300 focus:outline-none hover:bg-gray-100 focus:ring-4 focus:ring-gray-100 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2"
      >
        Войти
      </button>
    </form>
    <RouterLink
      class="text-lg text-center font-medium text-gray-700 hover:underline hover:decoration-solid"
      to="/signup"
      >Зарегистрироваться</RouterLink
    >
  </main>
</template>
