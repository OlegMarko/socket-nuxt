<template>
  <v-layout
    column
    justify-center
    align-center
  >
    <v-flex xs12 sm8>
      <v-card min-width="400">
        <v-card-title>
          <h1>Nuxt Chat App</h1>
        </v-card-title>
        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation>
            <v-text-field
              v-model="name"
              :rules="nameRules"
              :counter="16"
              label="Your Name"
              required
            ></v-text-field>
            <v-text-field
              v-model="room"
              :rules="roomRules"
              label="Room ID"
              required
            ></v-text-field>

            <v-btn class="mr-4" @click="submit">Start Messaging</v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-flex>
  </v-layout>
</template>

<script>
    import { validationMixin } from 'vuelidate'
    import { required, maxLength, email } from 'vuelidate/lib/validators'
    import { mapMutations } from 'vuex'

    export default {
        layout: 'empty',
        head: {
            title: 'Chat App'
        },
        mixins: [validationMixin],
        data: () => ({
            valid: true,
            name: '',
            nameRules: [
                v => !!v || 'Name is required',
                v => (v && v.length <= 16) || 'Name must be at most 16 characters long',
            ],
            room: '',
            roomRules: [
                v => !!v || 'Room ID is required',
            ],
        }),

        sockets: {
            connect: function () {
                console.log('Client IO connected');
            }
        },

        methods: {
            ...mapMutations(['setUser']),
            submit () {
                if (this.$refs.form.validate()) {
                    const user = {
                        name: this.name,
                        room: this.room
                    }

                    this.$socket.emit('userJoined', user, data => {
                        if (typeof data === 'string') {
                            return console.error(data)
                        }

                        user.id = data.userId

                        this.setUser(user)
                        this.$router.push('chat')
                    })
                }
            }
        },
    }
</script>

