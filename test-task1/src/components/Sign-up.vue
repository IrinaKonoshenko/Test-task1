<!-- <script setup lang="ts">
defineProps<{
  msg: string
}>()
</script> -->

<template>
  <div class="registration-form">
    <h1>Регистрация</h1>
    <form @submit.prevent="onSubmit">
      <div class="form-group">
        <label for="username">Имя пользователя</label>
        <input
          type="text"
          id="username"
          v-model="form.username"
          placeholder="Введите имя пользователя"
        />
        <span v-if="errors.username">{{ errors.username }}</span>
      </div>

      <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" v-model="form.email" placeholder="Введите email" />
        <span v-if="errors.email">{{ errors.email }}</span>
      </div>

      <div class="form-group">
        <label for="password">Пароль</label>
        <input type="password" id="password" v-model="form.password" placeholder="Введите пароль" />
        <span v-if="errors.password">{{ errors.password }}</span>
      </div>

      <button type="submit">Зарегистрироваться</button>
    </form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue'

interface FormData {
  username: string
  email: string
  password: string
}

interface FormErrors {
  username?: string
  email?: string
  password?: string
}

const form = reactive<FormData>({
  username: '',
  email: '',
  password: '',
})

const errors = reactive<FormErrors>({})

const validate = (): boolean => {
  errors.username = form.username ? '' : 'Имя пользователя обязательно'
  errors.email = form.email.includes('@') ? '' : 'Введите корректный email'
  errors.password = form.password.length >= 6 ? '' : 'Пароль должен содержать не менее 6 символов'

  return !errors.username && !errors.email && !errors.password
}

const onSubmit = () => {
  if (validate()) {
    console.log('Данные формы:', form)
    alert('Регистрация прошла успешно!')
  }
}
</script>

<style scoped>
.registration-form {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: #f9f9f9;
}

.form-group {
  margin-bottom: 15px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}

button {
  display: block;
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}

span {
  color: red;
  font-size: 12px;
}
</style>
