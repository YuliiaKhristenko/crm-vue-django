<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <div class="is-flex is-flex-direction-row	is-justify-content-space-between">
          <h1 class="title">Leads</h1>

          <router-link to="/dashboard/leads/add" v-if="$store.state.team.max_leads > num_leads" class="button is-info">Add lead</router-link>

          <div class="notification is-danger" v-else>
            <p>You have reached the top of your limitations. Please upgrade!</p>
          </div>
        </div>

        <hr>
        <form @submit.prevent="getLeads">
          <div class="field has-addons">
            <div class="control">
              <input class="input" type="text" placeholder="Find a lead" v-model="query">
            </div>
            <div class="control">
              <button class="button is-info">
                Search
              </button>
            </div>
          </div>
        </form>
      </div>

      <div class="column is-12">
        <table class="table is-fullwidth">
          <thead>
            <tr>
              <th>Company</th>
              <th>Contact person</th>
              <th>Assigned to</th>
              <th>Status</th>
              <th></th>
            </tr>
          </thead>

          <tbody>
            <tr v-for="lead in leads" :key="lead.id">
              <td>{{ lead.company }}</td>
              <td>{{ lead.contact_person }}</td>
              <td>
                <template v-if="lead.assigned_to">{{ lead.assigned_to.first_name }} {{ lead.assigned_to.last_name
                }}</template>
              </td>
              <td>{{ lead.status }}</td>
              <td>
                <router-link :to="{ name: 'Lead', params: { id: lead.id } }">Details</router-link>
              </td>
            </tr>
          </tbody>
        </table>

        <div class="buttons pagination is-centered">
          <button class="button pagination-previous is-light" v-if="showPreviousButton"
            @click="goToPreviousPage()">Previous</button>
          <button class="button pagination-next is-light" v-if="showNextButton" @click="goToNextPage()">Next</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Leads',
  data() {
    return {
      leads: [],
      showNextButton: false,
      showPreviousButton: false,
      currentPage: 1,
      query: '',
      num_leads: 0
    }
  },
  mounted() {
    this.getLeads()
  },
  methods: {
    goToNextPage() {
      this.currentPage += 1
      this.getLeads()
    },
    goToPreviousPage() {
      this.currentPage -= 1
      this.getLeads()
    },
    async getLeads() {
      this.$store.commit('setIsLoading', true)
      this.showNextButton = false
      this.showPreviousButton = false
      
      await axios
      .get(`/api/v1/leads/`)
      .then(response => {
        console.log('num_leads response', response.data);
        this.num_leads = response.data.count})

      await axios
      .get(`/api/v1/leads/?page=${this.currentPage}&search=${this.query}`)
      .then(response => {
        console.log('lead response from server',response.data);
        this.leads = response.data.results

        if (response.data.next) {
          this.showNextButton = true
        }
        if (response.data.previous) {
          this.showPreviousButton = true
        }
      })
      .catch(error => {
        console.log(error);
      })

      this.$store.commit('setIsLoading', false)
    },
    // submitForm() {
    //   console.log(this.query);
    // },
  }
}
</script>

<style lang="scss" scoped></style>