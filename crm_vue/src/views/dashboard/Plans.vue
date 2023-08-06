<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
         <div class="is-flex is-flex-direction-row	is-justify-content-space-between">
          <h1 class="title">Plans</h1>
          <button @click="cancelPlan()" class="button is-danger">Cancel current plan</button>
         </div>
      </div>

      <div class="column is-6">
        <div class="box">
          <div class="is-flex is-flex-direction-row	is-justify-content-space-between">
            <div class="is-flex is-flex-direction-column">
              <h2 class="subtitle">Free Plan</h2>
              <p>Max 5 clients</p>
              <p>Max 5 leads</p>
            </div>
            <div class="is-flex is-flex-direction-column is-align-items-center">
              <h4 class="is-size-3">$0</h4>
              <button @click="subscribe('free')" class="button is-primary mt-3">Subscribe</button>
            </div>
          </div>
        </div>
      </div>

      <div class="column is-6">
        <div class="box">
          <div class="is-flex is-flex-direction-row	is-justify-content-space-between">
            <div class="is-flex is-flex-direction-column">
              <h2 class="subtitle">Premium Plan</h2>
              <p>Max 50 clients</p>
              <p>Max 50 leads</p>
            </div>
            <div class="is-flex is-flex-direction-column is-align-items-center">
              <h4 class="is-size-3">$25</h4>
              <button @click="subscribe('premium')" class="button is-primary mt-3">Subscribe</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { toast } from 'bulma-toast'

export default {
  name: 'Plans',
  data() {
    return {}
  },
  mounted() {},
  methods: {
    async cancelPlan() {
      this.$store.commit('setIsLoading', true)
      await axios.post('/api/v1/teams/cancel_plan/ ').then(response => {
        console.log('Cancelled plan', response.data);
        this.$store.commit('setTeam', {
          'id': response.data.id,
          'name': response.data.name,
          'plan': response.data.plan.name,
          'max_leads': response.data.plan.max_leads,
          'max_clients': response.data.plan.max_clients,
        })
        toast({
          message: 'Subscription was cancelled!',
          type: 'is-danger',
          dismissible: true,
          pauseOnHover: true,
          duration: 4000,
          position: 'top-right'
        })
        this.$router.push('/dashboard/team')
      })
      this.$store.commit('setIsLoading', false)
    },
    async subscribe(plan) {
      this.$store.commit('setIsLoading', true)

      const data = {
        plan: plan
      }
      await axios
        .post(`/api/v1/teams/upgrade_plan/`, data)
        .then(response => {
          console.log('Upgraded plan', response.data)

          this.$store.commit('setTeam', {
            'id': response.data.id,
            'name': response.data.name,
            'plan': response.data.plan.name,
            'max_leads': response.data.plan.max_leads,
            'max_clients': response.data.plan.max_clients,
          })
          toast({
            message: 'Subscription successfully completed!',
            type: 'is-success',
            dismissible: true,
            pauseOnHover: true,
            duration: 4000,
            position: 'top-right'
          })
          this.$router.push('/dashboard/team')
        })
        .catch(error => {
          console.log(error);
        })
      this.$store.commit('setIsLoading', false)
    },
  }
}
</script>

<style lang="scss" scoped>

</style>
