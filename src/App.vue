<template>
  <div id="app">
    <Header @search='dataRecovery' />
    <Main :userSearch='userSearch'/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Main from './components/Main.vue'
import '@fortawesome/fontawesome-free/js/all.js';
export default {
  name: 'App',
  components: {
    Header,
    Main
  },
  data:function(){
    return{
      userSearch:""
    }
  },
  methods:{
    dataRecovery(userSearch){
      this.userSearch=userSearch
      // console.log(this.userSearch);
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bootstrap/scss/bootstrap.scss';
@import '~@fortawesome/fontawesome-free/scss/fontawesome';
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>