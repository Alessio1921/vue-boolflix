<template>
  <header>
    <nav class="navbar navbar-dark p-0 bg-dark  navbar-expand-lg">
      <div class="container-fluid">
        <div class="nav-left d-flex">
          <a class="navbar-brand m-0 p-0"><img src="../assets/img/Netflix-Logo.svg" alt="logo netflix"></a>
          <div class="" >
            <ul class="navbar-nav flex-row align-items-center">
              <li class="nav-item">
                <a class="nav-link active"  href="#">Home</a>
              </li>
              <li class="nav-item d-none d-xl-block">
                <a class="nav-link" href="#">Serie Tv</a>
              </li>
              <li class="nav-item d-none d-xl-block">
                <a class="nav-link" href="#">Film</a>
              </li>
              <li class="nav-item d-none d-xl-block">
                <a class="nav-link" href="#">Originali</a>
              </li>
              <li class="nav-item d-none d-xl-block">
                <a class="nav-link" href="#">Aggiunti di recente</a>
              </li>
              <li class="nav-item d-none d-xl-block">
                <a class="nav-link" href="#">La mia lista</a>
              </li>
              <li class="ms-2 d-inline-block">
                <select class="form-select bg-black text-white" aria-label="Default select example" v-model="selected" @change="selectionGenre(selected)">
                  <option selected value="">Tutti i generi</option>
                  <option :value="genre.name" v-for="genre in genreList" :key="genre.id">{{genre.name}}</option>
                </select>
              </li>
            </ul>
          </div>
        </div>
        <div class="d-flex">
          <input class="form-control me-2 bg-black text-white" type="search" placeholder="Search" aria-label="Search" v-model="userSearch" @keyup.enter="$emit('search',userSearch)"> 
          <button class="btn btn-outline-danger rounded-circle " @click="$emit('search',userSearch)"><i class="fas fa-magnifying-glass"></i></button>
        </div>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'indexHeader',
  props:{'genreList':Array},
  data:function(){
    return{
      userSearch:"",
      selected:""
    }
  },
  methods:{
    selectionGenre(selected){
      console.log(selected);
      this.$emit('select', selected)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
header{
  position: fixed;
  z-index: 2;
  width: 100vw;
}
  nav{
    height: 10vh;
    .container-fluid{
      height: 100%;
      .nav-left{
        height: 100%;
        .navbar-brand{
        height: 100%;
          img{
            height: 100%;
          }
        }
        .navbar-nav{
          height: 10vh;
          line-height: 8vh;
          a{
            height: 10vh;
          }
        }
      }
    }
  }
</style>
