<template>
  <main>
    <ul>
      <li>{{userSearch}}</li>
      <li v-for="movie in listUserSearch" :key="movie.id">
        <div>
          {{movie.title}} 
        </div>
        <div>
          {{movie.original_title}} 
        </div>
        <div>
          {{movie.original_language}} 
        </div>
        <div>
          {{movie.vote_average}} 
        </div>
      </li>
    </ul>
  </main>
</template>

<script>
import axios from 'axios'
export default {
  name: 'indexMain',
  props: {
    'userSearch':String
  },
  data:function(){
    return{
      movieList:'',
      listUserSearch:"",
      // movieUser:''
    }
  },
  methods:{
    getApi(){
      axios
      .get('https://api.themoviedb.org/3/discover/movie?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&with_watch_monetization_types=flatrate')
      .then(response =>{
        this.movieList=response.data.results
      })
      .catch(error=>{
        console.log(error)
      })
    },
    getApiSearch(){
      if(this.userSearch!==""){
        axios
        .get("https://api.themoviedb.org/3/search/movie?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=en-US&page=1&query=" + this.userSearch.trim())
        .then(response =>{
          this.listUserSearch=response.data.results
        })
        .catch(error=>{
          console.log(error)
        })
      }
    }
  },
  created(){
    this.getApi()
  },
  computed:{
    callFunction(){
      return this.getApiSearch()
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>
