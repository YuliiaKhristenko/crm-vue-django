<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Edit {{ client.name }}</h1>
      </div>

      <div class="column is-12">
        <form @submit.prevent="submitForm">

          <div class="field">
            <label class="label">Name</label>
            <div class="control">
              <input type="text" name="company" class="input" placeholder="Apple" v-model="client.name">
            </div>
          </div>

          <div class="field">
            <label class="label">Contact person</label>
            <div class="control">
              <input type="text" class="input" placeholder="Tim Cook" v-model="client.contact_person">
            </div>
          </div>

          <div class="field">
            <label class="label">Email</label>
            <div class="control">
              <input type="email" class="input" placeholder="12345@email.com" v-model="client.email">
            </div>
          </div>

          <div class="field">
            <label class="label">Phone</label>
            <div class="control">
              <input type="text" class="input" placeholder="+18883575197" v-model="client.phone">
            </div>
          </div>

          <div class="field">
            <label>Website</label>
            <div class="control">
              <input type="text" class="input" placeholder="https://www.apple.com" v-model="client.website">
            </div>
          </div>


          <div class="field">
            <div class="control">
              <button class="button is-success is-medium">Update</button>
            </div>
          </div>

        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
  name: 'EditClient',
  data() {
    return {
      client: {},
    }
  },
  mounted() {
    this.getClient()
  },
  methods: {
    async getClient() {
      this.$store.commit('setIsLoading', true)

      const clientID = this.$route.params.id

      axios.get(`/api/v1/clients/${clientID}/`).then(response => {
        this.client = response.data
      })
        .catch(error => {
          console.log(error);
        })

      this.$store.commit('setIsLoading', false)
    },
    async submitForm() {
      this.$store.commit('setIsLoading', true)
      const clientID = this.$route.params.id

      axios
        .patch(`/api/v1/clients/${clientID}/`, this.client)
        .then(response => {
          toast({
            message: 'The client was update',
            type: 'is-success',
            dismissible: true,
            pauseOnHover: true,
            duration: 4000,
            position: 'top-right'
          })
          this.$router.push(`/dashboard/clients/${clientID}`)
        })
        .catch(error => {
          console.log(error);
        })
      this.$store.commit('setIsLoading', false)
    },
  }
}
</script>

<style lang="scss" scoped></style>