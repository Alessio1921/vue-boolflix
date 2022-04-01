<template>
  <main>
    <ul>
      <li>{{userSearch}}</li>
      <li v-for="movie in listUserSearch" :key="movie.id">
        <div>
          {{movie.title}} 
        </div>
        <div>
          <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`" :alt="`${movie.title} img`" v-if="movie.poster_path!==null">
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
      <li v-for="element in listSeriesSearch" :key="element.id">
        <div>
          {{element.name}} 
        </div>
        <div>
          <img :src="`http://image.tmdb.org/t/p/w500${element.poster_path}`" :alt="`${element.title} img`" v-if="element.poster_path!==null">
        </div>
        <div>
          {{element.original_name}} 
        </div>
        <div>
          {{element.original_language}} 
        </div>
        <div>
          {{element.vote_average}} 
        </div>
      </li>
    </ul>
  </main>
</template>

<script>
import axios from 'axios'
import json from '../../node_modules/emoji-flags/data.json'
export default {
  name: 'indexMain',
  props: {
    'userSearch':String
  },
  data:function(){
    return{
      movieList:'',
      listUserSearch:"",
      listSeriesSearch:"",
      flagList:json
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
    // movies serarch by user
    getApiSearch(){
      if(this.userSearch!==""){
        axios
        .get("https://api.themoviedb.org/3/search/movie?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT&page=1&query=" + this.userSearch.trim())
        .then(response =>{
          this.listUserSearch=response.data.results
          console.log(this.listUserSearch);
        })
        .catch(error=>{
          console.log(error)
        })
        axios
        .get("https://api.themoviedb.org/3/search/tv?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT&query=" + this.userSearch.trim())
        .then(response =>{
          this.listSeriesSearch=response.data.results
        })
        .catch(error=>{
          console.log(error)
        })
      }
    },
    flag(){
      if(this.listUserSearch!==""){
        this.flagList.forEach((flag) => {
          this.listUserSearch.forEach((lang) =>{
            if(flag.code.toLowerCase().includes(lang.original_language)){
              lang.original_language=flag.emoji
            }
          })
        })
      }
      if(this.listUserSearch!==""){
        this.flagList.forEach((flag) => {
          this.listSeriesSearch.forEach((lang) =>{
            if(flag.code.toLowerCase().includes(lang.original_language)){
              lang.original_language=flag.emoji
            }
          })
        })
      }
    }
  },
  created(){
    this.getApi()
  },
  computed:{
    callFunctionSearch(){
      return this.getApiSearch()
    },
    callFunctionFlag(){
      return this.flag()
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>
