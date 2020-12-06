<template>
    <div class = "user-profile">
        <div class = "user-profile__user-panel">
            <h1>@{{ user.username }}</h1>
            <div class = "user-profile__admin-badge" v-if = "user.isAdmin">
                Admin
            </div>
            <div class = "user-profile__admin-badge" v-else>
                Not Admin
            </div>
            <div class = "user-profile__follower-count">
                <strong>Followers:</strong> {{ followers }}
            </div>
        </div>
        <div class = "user-profile__twoots-wrapper">
            <div class = "user-profile__twoot" v-for = "twoot in user.twoots" v-bind:key="twoot.id"> 
                {{ twoot.content }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "UserProfile",
    data () {
    return {
      followers: 0,
      user: {
        id: 1,
        username: 'relativeread',
        firstName: 'Edson',
        lastName: 'Ayllon',
        email: 'mail@edsonayllon.com',
        isAdmin: true,
        twoots: [
          { id: 1, content: 'Twotter is amazing!' },
          { id: 2, content: "Don't forget to follow" }
        ]
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
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
}

.user-profile__admin-badge {
    background: #454955;
    border-radius: 6px;
    margin-right: auto;
    padding: 4px 6px;
    color: white;
    font-weight: bold;
}
</style>