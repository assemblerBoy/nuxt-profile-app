<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <form>
      <v-alert v-if="isError.errorAlert" type="error">Неверный логин или пароль!</v-alert>
      <v-text-field
        v-model="name"
        :error-messages="loginErrors"
        label="Логин"
        :error="isError.login ? true : false"
        required
        @input="$v.name.$touch(), formError('',isError, 'name')"
        @blur="$v.name.$touch(), formError( '',isError, 'name')"
      ></v-text-field>
      <v-text-field
        v-model="password"
        :error-messages="passwordErrors"
        :error="isError.password ? true : false"
        label="Пароль"
        required
        @input="$v.password.$touch(), formError(' ', isError, 'password')"
        @blur="$v.password.$touch(), formError(' ', isError, 'password')"
      ></v-text-field>
      
        <v-btn class="mr-4" @click="submit">Войти</v-btn>
        <v-btn @click="clear">Очистить</v-btn>
    </form>
  </v-layout>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, maxLength} from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],

  validations: {
    name: { required },
    password: { required }
  },

  data: () => ({
    name: ' ',
    password: ' ',
    isError: {
      name: false,
      password: false,
      errorAlert: false
    }
  }),

  computed: {
    loginErrors () {
      const errors = []
      if (!this.$v.name.$dirty) return errors
      !this.$v.name.required && errors.push('Обязательный параметр')
      return errors
    },
    passwordErrors () {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      !this.$v.password.required && errors.push('Обязательный параметр')
      return errors
    }
  },

  methods: {

    async submit () {
      if(this.login !== ' ' && this.password !== ' ') {

        const data = await this.$axios.$get('data.json')

        if(data.login === this.name && data.password === this.password) {

          this.$router.push('/profile/')

        } else {
          this.formError(true)
        }
      }   
    },

    clear () {
      this.$v.$reset()
      this.name = ''
      this.password = ''
      this.formError(false)
    },
    
    // Функция принимает 3 параметра
    // -- булевое значение устанавливает показать/скрыть ошибки 
    // -- объект содержащий в себе информацию о состоянии ошибок
    // -- ключи объекта, для гибкой стилизации и избежания повторяющегося кода 
    formError(boolean, errorObj, key ) {
      if(boolean !== ' '){
        this.isError.login = boolean
        this.isError.password = boolean
        this.isError.errorAlert = boolean
      }  else{
        this.isError[key] = false
        this.isError.errorAlert = false
      }
    }
  }
}
</script>

<style scoped>
  form {
    width: 330px !important;
  }
</style>