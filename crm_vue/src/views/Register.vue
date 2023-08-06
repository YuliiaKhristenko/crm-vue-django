<template>
  <div class="container">
    <div class="columns">
      <div class="column is-4 is-offset-4">
        <h1 class="is-size-2 has-text-centered has-text-weight-medium mb-5">Register</h1>

        <form @submit.prevent="submitForm">

          <div class="field">
            <div class="control">
              <input class="input" name="input" type="email" placeholder="Email" v-model="username">
            </div>
          </div>

          <div class="field">
            <div class="control">
              <input class="input" name="password1" type="password" placeholder="Password" v-model="password1">
            </div>
          </div>

          <div class="field">
            <div class="control">
              <input class="input" name="password2" type="password" placeholder="Confirm password" v-model="password2">
            </div>
          </div>

          <div class="field mt-5">
            <div class="control">
              <button class="button is-success is-normal is-fullwidth">
                Register
              </button>
            </div>
          </div>


          <div class="notification is-danger" v-if="errors.length">
            <p v-for="error in errors" :key="error">{{ error }}</p>
          </div>

        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import {toast} from 'bulma-toast';

export default {
  name: 'Register',
  data() {
    return {
      username: '',
      password1: '',
      password2: '',
      errors: []
    }
  },
  methods: {
    async submitForm() {
      this.errors = []

      if (this.username === '') {
        this.errors.push('The username is missing')
      }

      if (this.password1 === '') {
        this.errors.push('The password is too short')
      }

      if (this.password1 !== this.password2) {
        this.errors.push('The password are not matching')
      }

      if (!this.errors.length) {
        this.$store.commit('setIsLoading', true)

        const formData = {
          username: this.username,
          password: this.password1
        }

        await axios.post('/api/v1/users/', formData).then(response => {
          toast({
            message: 'Account was created. Please login',
            type: 'is-success',
            dismissible: true,
            pauseOnHover: true,
            duration: 2000,
            position: 'top-right'
          })

          this.$router.push('/login')
        })
          .catch(error => {
            if (error.response) {
              for (const property in error.response.data) {
                this.errors.push(`${property}: ${error.response.data[property]}`)
              }
            } else if (error.message) {
              this.errors.push('Something went wrong. Please try again')
            }
          })
        this.$store.commit('setIsLoading', false)
      }
    },
  }
}
</script>

<style lang="scss" scoped>

</style>