<template>
  <div class="margin">
    <v-layout
      align-center
      class="mt-12"
      column
    >
      <v-flex
        sm8
        xs12


      >
        <v-card min-width="400">
          <v-snackbar
            :timeout="6000"
            top
            v-model="snackbar"
          >
            {{ message }}
            <v-btn
              @click="snackbar = false"
              dark
              text
            >
              Закрыть
            </v-btn>
          </v-snackbar>
          <v-card-title><h1>Войти в Чат</h1></v-card-title>
          <v-card-text>
            <v-form
              lazy-validation
              ref="form"
              v-model="valid"

            >
              <v-text-field
                :counter="16"
                :rules="nameRules"
                label="Имя"
                required
                v-model="name"
              ></v-text-field>

              <v-text-field
                :rules="roomRules"
                label="Ваша комната"
                required
                v-model="room"
              ></v-text-field>


              <v-btn
                :disabled="!valid"
                @click="submit"
                color="primary"
              >
                Войти
              </v-btn>

            </v-form>
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </div>
</template>

<script>
  import {mapMutations} from 'vuex'

  export default {
    head: {
      title: 'Добро пожаловать в Чат!'
    },
    layout: 'empty',
    sockets: {
      connect() {
        console.log('client connected')
      }
    },
    data: () => ({
      valid: true,
      snackbar: false,
      message: '',
      name: '',
      nameRules: [
        v => !!v || 'Введите имя',
        v => (v && v.length <= 16) || 'Имя не должно привышать 16 символов.',
      ],
      room: '',
      roomRules: [v => !!v || 'Введите комнату'],
    }),
    mounted() {
      const {message} = this.$route.query
      if (message === 'noUser') {
        this.message = 'Введите данные';
      } else if (message === 'leftChat') {
        this.message = 'Вы вышли из чата!';
      }
      this.snackbar = !!this.message;
    },
    methods: {
      ...mapMutations(["setUser"]),
      submit() {
        this.$refs.form.validate()
        const user = {
          name: this.name,
          room: this.room,
        }

        this.$socket.emit("userJoined", user, data => {
          if (typeof data === 'string') {
            console.log('Error');
          } else {
            user.id = data.userID;
            this.setUser(user);
            this.$router.push("/chat");
          }
        })


      },
    },
  }
</script>

<style lang="sass" scoped>
.margin
  margin-top: 50px
</style>
