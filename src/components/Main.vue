<template>
  <main class="bg-black">
    <div class="container-fluid">
      <h1 class="text-center text-white">Film</h1>
      <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 " v-if="userSearch==''" >
        <div class="col my-2" v-for="movie in listGenreFilterHomeFilm" :key="movie.id">
          <div class="my-card position-relative" >
            <img :src="movie.poster_path!==null ? `http://image.tmdb.org/t/p/w500${movie.poster_path}` : 'https://www.publicdomainpictures.net/pictures/280000/nahled/not-found-image-15383864787lu.jpg'" :alt="`${movie.title} img`" >
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
                <div>{{movie.popularity}}</div>
              </li>
            </ul>
          </div>
        </div>
        <div class="col-12 col-sm-12 col-md-12 col-lg-12 col-xl-12">
          <div class="text-center">
            <button class="btn btn-outline-danger py-2 px-5 my-5 mx-auto" @click="getApiHome(page++)">altro</button>
          </div>
        </div>
      </div> 
      <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 " v-if="userSearch!=''">
        <div class="col my-2" v-for="movie in listGenreFilterFilm" :key="movie.id" >
          <div class="my-card position-relative" >
            <img :src="movie.poster_path!==null ? `http://image.tmdb.org/t/p/w500${movie.poster_path}` : 'https://www.publicdomainpictures.net/pictures/280000/nahled/not-found-image-15383864787lu.jpg'" :alt="`${movie.title} img`" >
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
                <div>{{movie.popularity}}</div>
              </li>
            </ul>
          </div>
        </div>
        <div class="col-12 col-sm-12 col-md-12 col-lg-12 col-xl-12">
          <div class="text-center">
            <button class="btn btn-outline-danger py-2 px-5 my-5 mx-auto" @click="getApiSearch(page++)">altro</button>
          </div>
        </div>
      </div>
      <!-- series tv -->
      <h1 class="text-center text-white" v-if="userSearch!=''">Serie Tv</h1>
      <div class="row row-cols-2 row-cols-sm-3 row-cols-md-4 row-cols-lg-5 " v-if="userSearch!==''">
        <div class="col my-2" v-for="element in listGenreFilterSeries" :key="element.id">
          <div class="my-card position-relative">
            <img :src="element.poster_path!==null ? `http://image.tmdb.org/t/p/w500${element.poster_path}` : 'https://www.publicdomainpictures.net/pictures/280000/nahled/not-found-image-15383864787lu.jpg'" :alt="`${element.name} img`">
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
                <div>{{element.popularity}}</div>
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
    'userSearch':String,
    'selectedGenre':String
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
      actors:"",
      page:2
    }
  },
  methods:{
    // start call axios 
    getApiHome(page){
      axios
      .get(`https://api.themoviedb.org/3/discover/movie?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT&sort_by=popularity.desc&include_adult=false&include_video=false&page=${page}&with_watch_monetization_types=flatrate`)
      .then(response =>{
        this.movieList=response.data.results
        this.movieList.forEach(element => {
          // I replace the popularity element by calling the function
            element.popularity=this.getApiActor(element);
          });
      })
      .catch(error=>{
        console.log(error)
      })
    },
    // movies and series search by user
    getApiSearch(page){
      if(this.userSearch!==""){
        axios
        .get(`https://api.themoviedb.org/3/search/movie?api_key=d008d951e84fee160c9a8f2268d3e3a1&language=it-IT&page=${page}&query=` + this.userSearch.trim())
        .then(response =>{
          this.listUserSearch=response.data.results
          this.listUserSearch.forEach(element => {
          // I replace the popularity element by calling the function
            element.popularity=this.getApiActor(element);
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
          // I replace the popularity element by calling the function
            element.actorsList=this.getApiActor(element);
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
        this.genreList=response.data.genres;
        this.$emit('genres',this.genreList);
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
        film.popularity= this.actors;
        console.log("attori");
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
      this.flagList.forEach((flag) => {
          this.movieList.forEach((lang) =>{
            if(flag.code.toLowerCase().includes(lang.original_language)){
              lang.original_language=flag.emoji
            }
          })
        })
    },
    genreConvert(){
      if(this.listUserSearch!=""){
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
      this.movieList.forEach(element => {
        this.genreList.forEach(genre => {
          if(element.genre_ids.includes(genre.id)){
            this.genreTemp+=genre.name+", "
          }
          else if(element.genre_ids.includes(genre.name)){
            this.genreTemp=element.genre_ids
          }
        });
        element.genre_ids=this.genreTemp;
        this.genreTemp="";
      });
    }
  },
  created(){
    this.getApiHome(1);
    this.getApiGenre();
  },
  beforeDestroy(){
    this.genreConvert()
  },
  computed:{
    // filter list film by genre
    listGenreFilterHomeFilm(){
      if(this.selectedGenre==""){
        return this.movieList
      }
      return this.movieList.filter(film => film.genre_ids.includes(this.selectedGenre))
    },
    listGenreFilterFilm(){
      if(this.selectedGenre==""){
        return this.listUserSearch
      }
      return this.listUserSearch.filter(film => film.genre_ids.includes(this.selectedGenre))
    },
      // filter list series by genre
    listGenreFilterSeries(){
      if(this.selectedGenre==""){
        return this.listSeriesSearch
      }
      return this.listSeriesSearch.filter(film => film.genre_ids.includes(this.selectedGenre))
    },
  },
  watch:{
    userSearch:function(){
      this.genreConvert();
      this.getApiSearch(1);
    },
    listUserSearch:function(){
      this.genreConvert()
      },
    listSeriesSearch:function(){
      this.flag()
    },
    genreList:function(){
      this.genreConvert()
    },
    movieList:function(){
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
  margin-top: 10vh;
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
            opacity: 0;
          }
          ul{
            display: block;
            top: 0;
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