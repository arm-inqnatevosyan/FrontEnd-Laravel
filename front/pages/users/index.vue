<template>
  <div>
    <NavBars />
    <div class="mt-7 w-full sm:grid-cols-1 md:grid-cols-3 lg:grid grid-cols-3">
      <SeePost
        v-for="user in users"
        :id="user.id"
        :key="user.id"
        :email="user.email"
        :name="user.name"
        :subject="user.subject"
        :comments="user.comments"
      />
    </div>
  </div>
</template>

<script>
import NavBars from '@/components/NavBars'
import SeePost from '@/components/SeePost'
export default {
  components: { NavBars, SeePost },
  middleware: ['auth'],
  data () {
    return {
      users: [],
      content: ''
    }
  },
  async mounted () {
    await this.$axios.get('/api/postman/csrf')
    const response = await this.$axios.get('/api/contacts')
    this.users = response.data
  },
  methods: {
    async openUserProfile (user) {
      await this.$axios.get('/api/postman/csrf')
      await this.$router.push('/api/contacts' + '/' + `${user}`)
      if (user) {
        user = !user
      }
    },
    async addComment (id, title) {
      await this.$axios.get('/api/postman/csrf')
      return await this.$axios.post('/api/contacts/comments', { contact_id: id, title })
    }
  }
}
</script>
