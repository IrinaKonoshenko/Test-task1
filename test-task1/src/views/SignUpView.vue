<script setup lang="ts">
import Input from '@/components/Input.vue'
import { reactive } from 'vue'
import { z } from 'zod'

const initialErrors = {
  firstName: undefined,
  lastName: undefined,
  email: undefined,
  password: undefined,
  repeatPassword: undefined,
}

const initialValues = {
  firstName: '',
  lastName: '',
  email: '',
  password: '',
  repeatPassword: '',
}

const schema = z
  .object({
    firstName: z.string().min(1).max(64),
    lastName: z.string().min(1).max(64),
    email: z.string().email().min(1).max(64),
    password: z.string().regex(/^(?=(.*[A-Z]))(?=(.*\d))(?=(.*[!@#$%^&*(),.?":{}|<>])).{8,}$/, {
      message:
        'Password must be at least 8 characters long, with at least one uppercase letter, one number, and one special character.',
    }),
    repeatPassword: z
      .string()
      .regex(/^(?=(.*[A-Z]))(?=(.*\d))(?=(.*[!@#$%^&*(),.?":{}|<>])).{8,}$/, {
        message:
          'Password must be at least 8 characters long, with at least one uppercase letter, one number, and one special character.',
      }),
  })
  .refine((data) => data.password === data.repeatPassword, {
    message: 'Passwords do not match',
    path: ['repeatPassword'],
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

    return
  }

  console.log('Ура вы зарегались')
}
</script>

<template>
  <main class="max-w-80 mx-auto flex flex-col gap-6">
    <div class="text-lg font-medium text-gray-900">Пройдите регистрацию:</div>
    <form @submit.prevent="onSubmit" class="flex flex-col gap-5">
      <Input
        v-model="state.values.firstName"
        label="Введите имя"
        :error="state.errors?.firstName"
        placeholder="George"
        type="text"
      />
      <Input
        v-model="state.values.lastName"
        label="Введите фамилию"
        :error="state.errors?.lastName"
        placeholder="Bond"
        value=""
        type="text"
      />
      <Input
        v-model="state.values.email"
        label="Введите e-mail"
        :error="state.errors?.email"
        placeholder="example@example.com"
        value=""
        type="email"
      />
      <Input
        v-model="state.values.password"
        label="Введите пароль"
        :error="state.errors?.password"
        placeholder="123456"
        value=""
        type="password"
      />
      <Input
        v-model="state.values.repeatPassword"
        label="Повторите пароль"
        :error="state.errors?.repeatPassword"
        placeholder="123456"
        value=""
        type="password"
      />
      <div class="h-captcha" data-sitekey="ES_8356340851f8437a87fe78102c68d818"></div>
      <button
        type="submit"
        class="text-gray-900 bg-white border border-gray-300 focus:outline-none hover:bg-gray-100 focus:ring-4 focus:ring-gray-100 font-medium rounded-lg text-sm px-5 py-2.5 me-2 mb-2"
      >
        Далее
      </button>
    </form>
    <RouterLink
      class="text-center text-lg font-medium text-gray-700 hover:underline hover:decoration-solid"
      to="/signin"
      >Войти</RouterLink
    >
  </main>
</template>
