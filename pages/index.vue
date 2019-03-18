<template>
  <div style="padding-top: 10vh;">
    <table class="table">
      <thead>
        <tr>
          <th>Name</th>
          <th>Consecutive win</th>
          <th>Dates</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in userList.data" :key="user.ID">
          <td>{{ user.name }}</td>
          <td>{{ user.wins }}</td>
          <td>{{ user.created_at | createdAt }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
// TODO: Display list
export default {
  filters: {
    /**
     * Filter that format the created_at attribute
     * @param date
     * @returns {string}
     */
    createdAt(date) {
      return date.substr(0, 10)
    }
  },
  data() {
    return {
      userList: []
    }
  },
  mounted() {
    this.fetchUsers()
  },
  methods: {
    /**
     * Fetch a list of users
     * @returns {Promise<void>}
     */
    async fetchUsers() {
      this.userList = await this.$axios.$get('/users/').catch(err => {
        this.err = err
        this.$sentry.captureException(new Error(err.response))
      })
    }
  }
}
</script>
