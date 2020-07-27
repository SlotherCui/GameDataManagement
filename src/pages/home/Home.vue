<template>
  <div>
    <home-header @search_name ="getSearchName"></home-header>
    <home-body :search_name="search_name" ref="body"></home-body>
    <home-footer></home-footer>
  </div>
</template>

<script>
import HomeHeader from './components/Header'
import HomeBody from './components/Body'
import HomeFooter from './components/Footer'
export default {
  name: 'Home',
  components: {
    HomeHeader,
    HomeBody,
    HomeFooter
  },
  data () {
    return {
      search_name: ''
    }
  },
  methods: {
    getSearchName (msg) {
      this.search_name = msg
      this.$refs.body.getGameInfo(1, this.search_name)
    }
  }
}
</script>

<style lang="stylus">
</style>
