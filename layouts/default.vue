<template>
  <v-app app dark>
    <v-navigation-drawer app mobile-break-point="650px" v-model="drawer">
      <v-list subheader>
        <v-subheader>Участники чата</v-subheader>
        <v-list-item
          :key="user.id"
          @click.prevent
          v-for="u in users"
        >


          <v-list-item-content>
            <v-list-item-title>{{u.name}}</v-list-item-title>
          </v-list-item-content>

          <v-list-item-icon>
            <v-icon :color="u.id === user.id    ? 'primary' : 'grey'">mdi-chat-outline</v-icon>
          </v-list-item-icon>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar absolute
               app
               color="	#181818"
               elevate-on-scroll
               scroll-target="#scrolling-techniques-7"
    >
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-btn @click="exit" icon>
        <v-icon dark>mdi-arrow-left</v-icon>
      </v-btn>
      <v-toolbar-title>Чат комнаты {{user.room}}</v-toolbar-title>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <div style="height: 100%">
        <nuxt/>
      </div>
    </v-content>
  </v-app>
</template>

<script>
  import {mapMutations, mapState} from "vuex";

  export default {

    data: () => ({

      drawer: true,
    }),
    methods: {
      ...mapMutations(['clearData']),
      exit() {
        this.$socket.emit('userLeft', this.user.id, ()=>{
          this.$router.push("/?message=leftChat");
          this.clearData();
        })

      },
    },
    computed: mapState(["user", "users"]),
  };
</script>
