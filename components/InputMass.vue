<template>
  <div class="input">
    <v-col cols="12" >
      <v-text-field
        v-model="text" @keydown.enter="send"
        filled
        label="Ваше сообщение"
      ></v-text-field>
    </v-col>
  </div>
</template>

<script>
export default {
  data: () =>({
    text: '',
  }),
  methods: {
    send() {
      this.$socket.emit(
        'createMessage',
        {
          text: this.text,
          id: this.$store.state.user.id
        },
        data => {
          if (typeof data === "string") {
            console.error(data);
          } else {
            this.text = "";
          }
        }
      );
    }
  }
}
</script>

<style lang="sass" scoped>
  .input
    margin-bottom: 1rem
</style>
