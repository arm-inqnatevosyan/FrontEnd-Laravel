<template>
  <div>
    <NavBars />
    <h1 class="mt-7 text-3xl font-bold text-sky-600 text-center">
      Bad Category Posts
    </h1>
    <div class="mt-7 w-full sm:grid-cols-1 md:grid-cols-3 lg:grid grid-cols-3">
      <SeePost
        v-for="(user, index) in users.contacts"
        :id="user.id"
        :key="index"
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
    const response = await this.$axios.get('/api/categorys')
    this.users = response.data[1]
  },
  methods: {
    async addComment (id, title) {
      await this.$axios.get('/api/postman/csrf')
      return await this.$axios.post('/api/contacts/comments', { contact_id: id, title })
    }
  }
}
</script>
