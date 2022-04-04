<template>
  <main class="bg-black">
    <div class="container-fluid">
      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-4 row-cols-lg-5 ">
        <div class="col my-2" v-for="movie in listUserSearch" :key="movie.id">
          <div class="my-card position-relative">
            <img :src="`http://image.tmdb.org/t/p/w500${movie.poster_path}`" :alt="`${movie.title} img`" v-if="movie.poster_path!==null">
            <ul class="position-absolute text-white">
              <li>
                <span>Titolo:</span>{{movie.title}} 
              </li>
              <li>
                <span>Titolo Originale:</span>{{movie.original_title}} 
              </li>
              <li>
                <span>Lingua Originale:</span>{{movie.original_language}} 
              </li>
              <li>
                <span>Voto:</span> {{Math.round(movie.vote_average / 2)}} 
              </li>
              <li>
                <span>Trama:</span>{{movie.overview}} 
              </li>
            </ul>
          </div>
        </div>
      </div>
      <!-- series tv -->
      <div class="row row-cols-1 row-cols-sm-2 row-cols-md-4 row-cols-lg-5 ">
        <div class="col my-2" v-for="element in listSeriesSearch" :key="element.id">
          <div class="my-card position-relative">
            <img :src="`http://image.tmdb.org/t/p/w500${element.poster_path}`" :alt="`${element.title} img`" v-if="element.poster_path!==null">
            <ul class="position-absolute text-white">
              <li>
                <span>Titolo:</span>{{element.name}} 
              </li>
              <li>
                <span>Titolo Originale:</span>{{element.name}} 
              </li>
              <li>
                <span>Lingua Originale:</span>{{element.original_language}} 
              </li>
              <li>
                <span>Voto:</span> {{Math.round(element.vote_average / 2)}} 
              </li>
              <li>
                <span>Trama:</span>{{element.overview}} 
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
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
      flagList:json,
      prova:""
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
  watch:{
    userSearch:function(){
      this.getApiSearch()
      this.flag()
    }
  }
}
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
main{
  height: 100vh;
  overflow-y:auto ;
}
.container-fluid{
  height: 100%;
  .row{
    .col{
      .my-card{
        height: 100%;
        overflow-y: auto;
        ul{
          display: none;
          list-style: none;
          padding: 0.4rem;
          li{
            margin-bottom: 0.4rem;
            font-size: .8rem;
            span{
              font-weight: bold;
            }
          }
        }
        &:hover{
          background-color: black;
          img{
            display: none;
          }
          ul{
            display: block;
          }
        }
        img{
          height: 100%;
          width: 100%;
          object-fit: cover;
        }
      }
    }
  }
}
</style>