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
            <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot" :class="{ '--exceeded': newTwootCharacterCount > 180  }"> 
                <label for="newTwoot"><strong>New Twoot</strong> ({{ newTwootCharacterCount }}/180)</label>
                <textarea id="newTwoot" rows="4" v-model="newTwootContent" />
                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type: </strong></label>
                    <select id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>
                <button>
                    Twoot!
                </button>
            </form>
        </div>
        <div class = "user-profile__twoots-wrapper">
            <TwootItem 
                v-for="twoot in user.twoots" 
                :key="twoot.id" 
                :username="user.username" 
                :twoot="twoot"
                @favorite="toggleFavorite"
            /> 
        </div>
    </div>
</template>

<script>
import TwootItem from './TwootItem'

export default {
    name: "UserProfile",
    components: {
        TwootItem
    },
    data () {
    return {
        newTwootContent: '',
        selectedTwootType: 'instant',
        twootTypes: [
            { value: 'draft', name: 'Draft' },
            { value: 'instant', name: 'Instant Twoot' }
        ],
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
    },
    newTwootCharacterCount() {
        return this.newTwootContent.length
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
    },
    toggleFavorite(id) {
        console.log(`Favorited tweet with id ${id}`)
    },
    createNewTwoot() {
        if (this.newTwootContent && this.selectedTwootType !== 'draft') {
            this.user.twoots.unshift({
                id: this.user.twoots.length + 1,
                content: this.newTwootContent
            })
            this.newTwootContent = ''
        }
    }
  },
  mounted() {
      this.followUser()
  }
}
</script>

<style lang="scss" scoped>

.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;

    .user-profile__user-panel {
        display: flex;
        flex-direction: column;
        margin-right: 50px;
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid #DFE3E8;

        .user-profile__admin-badge {
            background: rebeccapurple;
            border-radius: 6px;
            margin-right: auto;
            padding: 4px 6px;
            color: white;
            font-weight: bold;
        }

        .user-profile__create-twoot {
            display: flex;
            flex-direction: column;
            padding-top: 16px;

            &.--exceeded {
                color: red;
                border-color: red;
                button {
                    border: none;
                    color: white;
                    background-color: red;
                }
            }
        }
    }

    .user-profile__twoots-wrapper {
        display: grid;
        grid-gap: 16px;
    }
}

</style>
