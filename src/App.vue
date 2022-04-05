<template>
  <div id="app">
    <Header @search='dataRecovery' :genreList='genreList' @select="genreSelect"/>
    <Main :userSearch='userSearch' @genres="genres" :selectedGenre="selectedGenre"/>
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
      userSearch:"",
      genreList:[],
      selectedGenre:""
    }
  },
  methods:{
    dataRecovery(userSearch){
      this.userSearch=userSearch
    },
    genres(genreList){
      this.genreList=genreList;
      console.log(this.genreList);
    },
    genreSelect(selected){
      this.selectedGenre=selected;
      // console.log(this.selectedGenre);
    }
  }
}
</script>

<style lang="scss">
@import '../node_modules/bootstrap/scss/bootstrap.scss';
@import '~@fortawesome/fontawesome-free/scss/fontawesome';
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  min-width: 580px;
}
</style>