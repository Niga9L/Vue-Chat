<template>
  <div class="c-wrap">
    <div class="c-chat" ref="block">
      <Message :key="message.text" :name="message.name" :text="message.text" :owner="message.id === user.id" v-for="message of messages"/>
    </div>
    <div class="c-form">
      <InputMass/>
    </div>
  </div>
</template>

<script>
  import {mapState} from 'vuex'
  import InputMass from "~/components/InputMass";
  import Message from "~/components/Message";

  export default {
    components: {
      Message,
      InputMass,
    },
    middleware: ['chat'],
    head() {
      return {
        title: `Комната ${this.user.room}`
      }
    },
    computed: mapState(['user', 'messages']),
    watch: {
      messages() {
        setTimeout(() => {
          this.$refs.block.scrollTop = this.$refs.block.scrollHeight;
        })

      }
    }
  }
</script>

<style lang="sass" scoped>
  .c-wrap
    height: 100%
    position: relative
    overflow: hidden

  .c-form
    margin-top: 1rem
    position: absolute
    bottom: 0
    left: 0
    right: 0
    max-height: 100px
    padding: 1rem
    background: #212121

  .c-chat
    position: absolute
    right: 0
    left: 0
    top: 0
    bottom: 100px
    padding: 1rem
    overflow-y: auto
</style>
