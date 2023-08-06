<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <div class="is-flex is-flex-direction-row	is-justify-content-space-between">
          <h1 class="title">{{ team.name }}</h1>

          <template v-if="team.created_by.id === parseInt($store.state.user.id)">
            <router-link :to="{ name: 'AddMember' }" class="button is-info">Add member</router-link>
          </template>
        </div>
        <hr>

        <div class="is-flex	is-justify-content-space-between is-align-items-center">
          <div>
            <p><strong>Plan:</strong> {{ $store.state.team.plan }}</p>
            <p><strong>Max clients:</strong> {{ $store.state.team.max_clients }}</p>
            <p><strong>Max leads:</strong> {{ $store.state.team.max_leads }}</p>
          </div>
          <div>
            <router-link :to="{ name: 'Plans' }" class="button is-info">Change plan</router-link>
          </div>
        </div>

        <hr>
      </div>
      <div class="column is-12">
        <h2 class="subtitle">Members</h2>
        <table class="table is-fullwidth">
          <thead>
            <tr>
              <th>Username</th>
              <th>Full Name</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="member in team.members" :key="member.id">
              <td>{{ member.username }}</td>
              <td>{{ member.first_name }} {{ member.last_name }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
  export default {
    name: 'Team',
    data() {
      return {
        team: {
          members: [],
          created_by: {},
        }
      }
    },
    mounted() {
      this.getTeam()
    },
    methods: {
      async getTeam() {
        this.$store.commit('setIsLoading', true)
        await axios
        .get('/api/v1/teams/get_my_team/')
        .then(response => {
          // console.log(response.data);
          this.team = response.data
        })
        .catch(error => {
          console.log(error);
        })

      this.$store.commit('setIsLoading', false)
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>