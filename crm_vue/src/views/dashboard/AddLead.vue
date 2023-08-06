<template>
  <div class="container">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Add lead</h1>
      </div>

      <div class="column is-12">
        <form @submit.prevent="submitForm">

          <div class="field">
            <label class="label">Company</label>
            <div class="control">
              <input type="text" name="company" class="input" placeholder="Apple" v-model="company">
            </div>
          </div>

          <div class="field">
            <label class="label">Contact person</label>
            <div class="control">
              <input type="text" class="input" placeholder="Tim Cook" v-model="contact_person">
            </div>
          </div>

          <div class="field">
            <label class="label">Email</label>
            <div class="control">
              <input type="email" class="input" placeholder="12345@email.com" v-model="email">
            </div>
          </div>

          <div class="field">
            <label class="label">Phone</label>
            <div class="control">
              <input type="text" class="input" placeholder="+18883575197" v-model="phone">
            </div>
          </div>

          <div class="field">
            <label>Website</label>
            <div class="control">
              <input type="text" class="input" placeholder="https://www.apple.com" v-model="website">
            </div>
          </div>

          <div class="field">
            <label class="label">Confidence</label>
            <div class="control">
              <input type="number" class="input" placeholder="0" v-model="confidence">
            </div>
          </div>

          <div class="field">
            <label class="label">Estimated value</label>
            <div class="control">
              <input type="number" class="input" placeholder="0" v-model="estimated_value">
            </div>
          </div>

          <div class="is-flex is-flex-direction-row is-justify-content-space-between is-align-items-center">
            <div class="is-flex is-flex-direction-row">
              <div class="field">
                <label>Status</label>
                <div class="control">
                  <div class="select">
                    <select v-model="status">
                      <option value="new">New</option>
                      <option value="contacted">Contacted</option>
                      <option value="inprogress">In progress</option>
                      <option value="lost">Lost</option>
                      <option value="won">Won</option>
                    </select>
                  </div>
                </div>
              </div>

              <div class="field mx-5">
                <label>Priority</label>
                <div class="control">
                  <div class="select">
                    <select v-model="priority">
                      <option value="low">Low</option>
                      <option value="medium">Medium</option>
                      <option value="high">High</option>
                    </select>
                  </div>
                </div>
              </div>
            </div>

            <div class="submit">
              <div class="field">
              <div class="control">
                <button class="button is-success is-medium">Submit</button>
              </div>
            </div>
            </div>
          </div>

        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import {toast} from 'bulma-toast'

export default {
  name: 'AddLead',
  data() {
    return {
      company: '',
      contact_person: '',
      email: '',
      phone: '',
      website: '',
      confidence: '',
      estimated_value: '',
      status: 'new',
      priority: 'low'
    }
  },
  methods: {
    async submitForm() {
      this.$store.commit('setIsLoading', true)
      const lead = {
        company: this.company,
        contact_person: this.contact_person,
        email: this.email,
        phone: this.phone,
        website: this.website,
        confidence: this.confidence,
        estimated_value: this.estimated_value,
        status: this.status,
        priority: this.priority

      }
      await axios.post('/api/v1/leads/', lead).then(response => {
        toast({
          message: 'The lead was update',
          type: 'is-success',
          dismissible: true,
          pauseOnHover: true,
          duration: 4000,
          position: 'top-right'
        })

        this.$router.push('/dashboard/leads')
      })
        .catch(error => {
          console.log(error);
        })
      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

<style lang="scss" scoped></style>