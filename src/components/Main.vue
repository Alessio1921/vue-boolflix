<template>
  <main class="bg-black">
    <div class="container-fluid">
      <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 ">
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
                <span>Voto:</span> <i class="fas fa-star" v-for="(n,index) in Math.round(movie.vote_average / 2)" :key="index">{{n}}</i>
              </li>
              <li>
                <span>Trama:</span>{{movie.overview}} 
              </li>
              <li>
                <span>Genere:</span> {{movie.genre_ids}}
              </li>
              <li>
                <span>Attori:</span>
                <div>{{movie.actorsList}}</div>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <!-- series tv -->
      <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 ">
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
                <span>Voto:</span> <i class="fas fa-star" v-for="(n,index) in Math.round(element.vote_average / 2)" :key="index">{{n}}</i>
              </li>
              <li>
                <span>Trama:</span>{{element.overview}} 
              </li>
              <li>
                <span>Genere:</span> {{element.genre_ids}}
              </li>
              <li>
                <span>Attori:</span>
                <div>{{element.actorsList}}</div>
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
      genreList:"",
      genreTemp:"",
      castList:"",
      actors:""
    }
  },
  methods:{
    // start call axios 
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
    // movies and series search by user
    getApiSearch(){
      if(this.userSearch!==""){
        axios
        .get("https://api.themoviedb.org/3/search/movie?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT&page=1&query=" + this.userSearch.trim())
        .then(response =>{
          this.listUserSearch=response.data.results
          this.listUserSearch.forEach(element => {
            element.actorsList="";
            // I call api actor and pass him the single film
            this.getApiActor(element);
          });
        })
        .catch(error=>{
          console.log(error)
        })
        axios
        .get("https://api.themoviedb.org/3/search/tv?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT&query=" + this.userSearch.trim())
        .then(response =>{
          this.listSeriesSearch=response.data.results
          this.listSeriesSearch.forEach(element => {
            element.actorsList="";
            // I call api actor and pass him the single film
            this.getApiActor(element);
          });
        })
        .catch(error=>{
          console.log(error)
        })
      }
    },
    getApiGenre(){
      axios
      .get('https://api.themoviedb.org/3/genre/movie/list?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT')
      .then(response=>{
        this.genreList=response.data.genres
      })
      .catch(error=>{console.log(error)}) 
    },
    getApiActor(film){
      axios
      .get(`https://api.themoviedb.org/3/movie/${film.id}/credits?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT`)
      .then(response=>{
        this.actors="";
        this.castList=response.data.cast;
          for (let i = 0; i < 5; i++) {
            this.actors+=this.castList[i].name+", "
          }
        film.actorsList= this.actors
      })
      .catch(error=>{console.log(error);})
    },
    // end call
    flag(){
      if(this.listUserSearch!==""){
        this.flagList.forEach((flag) => {
          this.listUserSearch.forEach((lang) =>{
            if(flag.code.toLowerCase().includes(lang.original_language)){
              lang.original_language=flag.emoji
            }
          })
        })
        this.flagList.forEach((flag) => {
          this.listSeriesSearch.forEach((lang) =>{
            if(flag.code.toLowerCase().includes(lang.original_language)){
              lang.original_language=flag.emoji
            }
          })
        })
      }
    },
    genreConvert(){
      if(this.listUserSearch!==""){
        this.listUserSearch.forEach(element => {
          this.genreList.forEach(genre => {
            if(element.genre_ids.includes(genre.id)){
              this.genreTemp+=genre.name+", "
            }
          });
          element.genre_ids=this.genreTemp;
          this.genreTemp="";
        });
      // series tv
        this.listSeriesSearch.forEach(element => {
          this.genreList.forEach(genre => {
            if(element.genre_ids.includes(genre.id)){
              this.genreTemp+=genre.name+", "
            }
          });
          element.genre_ids=this.genreTemp;
          this.genreTemp="";
        });
      }
    },
  },
  created(){
    this.getApi()
    this.getApiGenre()
  },
  computed:{
    callFunctionFlag(){
      return this.flag()
    },
    callGenreConvert(){
      return this.genreConvert()
    }
  },
  watch:{
    userSearch:function(){
      this.getApiSearch();
      // this.addActors()
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