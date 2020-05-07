<template>
  <section class="container">
    <div class="col-md-6 offset-md-3 mt-3">
      <form autocomplete="off" @submit.stop.prevent="handleSubmit">
        <div class="form-group">
          <label for="password">New Password</label>
          <b-form-input
            id="password"
            v-model="password"
            label="password"
            type="password"
            class="form-control"
            autofocus="true"
            placeholder="Enter your password"
            required
          />
        </div>
        <div class="form-group">
          <label for="password">Repeat Password</label>
          <b-form-input
            id="password"
            v-model="password"
            label="password"
            type="password"
            class="form-control"
            autofocus="true"
            placeholder="Enter your password"
            required
          />
        </div>
        <button
          :disabled="loading"
          type="submit"
          class="btn btn-primary btn-block mt-3"
        >
          Submit
        </button>
      </form>
    </div>
  </section>
</template>

<script>
import Strapi from 'strapi-sdk-javascript/build/main'
const apiUrl = process.env.API_URL || 'http://localhost:1337'
const strapi = new Strapi(apiUrl)
export default {
  data() {
    return {
      password: '',
      loading: false
    }
  },
  methods: {
    async handleSubmit() {
      try {
        let uri = window.location.search.substring(1)
        let params = new URLSearchParams(uri)
        console.log(params.get('code'))
        this.loading = true
        const response = await strapi.resetPassword(
          params.get('code'),
          this.password,
          this.password
        )
        this.loading = false
        this.setUser(response.user)
        this.$router.push('/')
      } catch (err) {
        this.loading = false
        alert(err.message || 'An error occurred.')
      }
    }
  }
}
</script>
