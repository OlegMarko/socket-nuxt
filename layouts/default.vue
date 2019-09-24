<template>
  <v-app app dark>
    <v-navigation-drawer app v-model="drawer">
      <v-list subheader>
        <v-subheader>Users list</v-subheader>

        <v-list-item
          v-for="u in users"
          :key="u.id"
          @click=""
        >
          <v-list-item-content>
            <v-list-item-title v-text="u.name"></v-list-item-title>
          </v-list-item-content>

          <v-list-item-icon>
            <v-icon :color="u.active ? 'deep-purple accent-4' : 'grey'">mdi-message</v-icon>
          </v-list-item-icon>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-btn icon @click="exit">
        <v-icon>mdi-backspace</v-icon>
      </v-btn>
      <v-toolbar-title>Chat room {{ user.room }}</v-toolbar-title>
    </v-app-bar>
    <v-content>
      <div>
        <nuxt/>
      </div>
    </v-content>
    <v-footer app></v-footer>
  </v-app>
</template>

<script>
  import { mapState, mapMutations } from 'vuex'

  export default {
      data: () => ({
          drawer: true,
          users: [
              { id: 1, active: true, name: 'Jason Oner'},
              { id: 2, active: true, name: 'Ranee Carlson'},
              { id: 3, name: 'Cindy Baker' },
              { id: 4, name: 'Ali Connors' },
          ],
      }),
      computed: mapState(['user']),
      methods: {
          ...mapMutations(['clearUser']),
          exit() {
              this.$router.push('/?message=leftChat')
              this.clearUser()
          }
      }
  };
</script>
