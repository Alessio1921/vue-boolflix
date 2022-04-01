<template>
  <div id="app">
    <Header @search='dataRecovery' />
    <Main :userSearch='userSearch'/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import Main from './components/Main.vue'

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
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
