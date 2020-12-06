<template>
  <div id="app">
    @{{ user.username }} - {{ fullName }}
    <strong>Followers</strong>
    {{ followers }}
    <button @click="followUser">Follow</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
    
  },
  data () {
    return {
      followers: 0,
      user: {
        id: 1,
        username: 'relativeread',
        firstName: 'Edson',
        lastName: 'Ayllon',
        email: 'mail@edsonayllon.com',
        isAdmin: true
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      // watch is like useEffect with state/hooks dependency
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower`)
      }
    }
  },
  methods: {
    followUser() {
      this.followers++
    }
  },
  mounted() {
      this.followUser()
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
