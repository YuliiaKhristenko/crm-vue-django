<template>
  <nav class="navbar is-dark">
    <div class="navbar-brand">
      <router-link to="/"
        class="is-size-4 has-text-weight-semibold has-text-light is-flex is-align-items-center ml-3">CRM</router-link>
    </div>

    <!-- <div class="navbar-menu"> -->

      <div class="navbar-end is-flex-mobile">
        <template v-if="$store.state.isAuthenticated">
          <router-link to="/dashboard/leads" class="navbar-item">Leads</router-link>
          <router-link to="/dashboard/clients" class="navbar-item">Clients</router-link>
          <router-link to="/dashboard/team" class="navbar-item">My Team</router-link>
        </template>

        <div class="navbar-item">
          <div class="buttons">
            <template v-if="!$store.state.isAuthenticated">
              <router-link to="/register" class="button is-success">Register</router-link>
              <router-link to="/login" class="button is-light">Login</router-link>
            </template>
            <template v-else>
              <router-link to="/dashboard/my-account" class="button is-success">My account</router-link>
            </template>
          </div>
        </div>
      </div>
    <!-- </div> -->
  </nav>
</template>

<script>
  export default {
    name: 'Navbar'
  }
</script>

<style lang="scss" scoped>

</style>