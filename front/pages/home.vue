<!-- Please remove this file from your project -->
<template>
  <div class="flex flex-col">
    <NavBars />
    <div class="flex flex-col w-1/3 mt-40 p-10 bg-sky-800 mx-auto h-my shadow-lg shadow-cyan-500/50">
      <h1 class="text-white font-bold font-sans mb-10 text-2xl">
        Add User
      </h1>
      <input
        id="input"
        v-model="name"
        class="border-solid border-2 border-sky-200 outline-none p-1"
        required
        type="text"
        placeholder="Name"
      >
      <input
        id="input"
        v-model="email"
        class="border-solid border-2 border-sky-200 outline-none p-1 mt-5"
        required
        type="email"
        placeholder="Email"
      >
      <input
        id="input"
        v-model="subject"
        class="border-solid border-2 border-sky-200 outline-none p-1 mt-5"
        required
        type="text"
        placeholder="Subject"
      >
      <button :disabled="(!name,!email,!subject)" style="padding: 5px;margin-top: 20px;background-color: grey;color:white;" @click="submit(name,email,subject)">
        Add User
      </button>
    </div>
    <button class="absolute bottom-2 right-2" @click="logout">
      <img src="/logout.png" class="w-12 h-12">
    </button>
  </div>
</template>

<script>
import NavBars from '../components/NavBars.vue'
export default {
  name: 'AddPost',
  components: { NavBars },
  middleware: ['auth'],
  data () {
    return {
      name: '',
      email: '',
      subject: ''
    }
  },
  methods: {
    async submit (name, email, subject) {
      const post = await this.$axios.post('/api/contact/submit', { name, email, subject })
      if (post !== '') {
        this.$router.push('/users')
      }
    },
    logout () {
      this.$cookiz.remove('jwt')
      this.$router.push('/')
    }
  }
}
</script>
