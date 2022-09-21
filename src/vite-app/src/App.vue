<template>
  <v-app>
    <Header/>
    <v-main>
      <TypingGame/>
    </v-main>
  </v-app>
</template>

<script>
import TypingGame from './components/TypingGame.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'
export default {
  name: 'App',

  components: {
    Header,
    TypingGame,
  },

}
</script>
