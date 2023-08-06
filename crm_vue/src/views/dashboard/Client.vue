<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <div class="is-flex	is-justify-content-space-between">
          <h1 class="title">{{ client.name }}</h1>
          <div class="buttons is-align-items-start">
            <router-link :to="{ name: 'EditClient', params: { id: client.id } }"
              class="button is-light">Edit</router-link>
            <button class="button is-danger" @click="deleteClient">Delete</button>
          </div>
        </div>


      </div>

      <div class="column is-6">
        <div class="box">
          <h2 class="subtitle">Details</h2>
          <p><strong>Created at:</strong> {{ client.created_at }}</p>
          <p><strong>Modified at:</strong> {{ client.modified_at }}</p>
        </div>
      </div>

      <div class="column is-6">
        <div class="box">
          <h2 class="subtitle">Contact information</h2>

          <p><strong>Contact person:</strong> {{ client.contact_person }}</p>
          <p><strong>Email:</strong> {{ client.email }}</p>
          <p><strong>Phone:</strong> {{ client.phone }}</p>
          <p><strong>Website:</strong> {{ client.website }}</p>
        </div>
      </div>

      <hr>

      <div class="column is-12">
        <div class="is-flex	is-justify-content-space-between">
          <h2 class="subtitle is-size-3">Notes</h2>
          <router-link :to="{ name: 'AddNote', params: { id: client.id } }" class="button is-info mb-4">Add Note</router-link>
        </div>


        <div class="box" v-for="note in notes" :key="note.id">
          <h3 class="is-size-4">{{ note.name }}</h3>
          <p>{{ note.body }}</p>
          <router-link :to="{ name: 'EditNote', params: { id: client.id, note_id: note.id } }"
            class="button is-light mt-4">Edit Note</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'Client',
  data() {
    return {
      client: {},
      notes: []
    }
  },
  mounted() {
    this.getClient()
  },
  methods: {
    async getClient() {
      this.$store.commit('setIsLoading', true)

      const clientID = this.$route.params.id

      await axios.get(`/api/v1/clients/${clientID}/`).then(response => {
        this.client = response.data
      })
        .catch(error => {
          console.log(error);
        })

      await axios.get(`/api/v1/notes/?client_id=${clientID}`).then(response => {
        this.notes = response.data
      })
      .catch(error => {
        console.log(error);
      })

      this.$store.commit('setIsLoading', false)
    },
    async deleteClient() {
      this.$store.commit('setIsLoading', true)

      const clientID = this.$route.params.id

      await axios.post(`/api/v1/clients/delete_client/${clientID}/`).then(response => {
        console.log('Deleted client', response.data);
      })
        .catch(error => {
          console.log(error);
        })
      this.$router.push('/dashboard/clients')

      this.$store.commit('setIsLoading', false)
    },
  }
}
</script>

<style lang="scss" scoped>
</style>