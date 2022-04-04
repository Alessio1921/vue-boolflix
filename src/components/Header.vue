<template>
  <header>
    <nav class="navbar navbar-dark p-0 bg-dark  navbar-expand-lg">
      <div class="container-fluid">
        <div class="nav-left d-flex">
          <a class="navbar-brand m-0 p-0"><img src="../assets/img/Netflix-Logo.svg" alt=""></a>
          <div class="" >
            <ul class="navbar-nav">
              <li class="nav-item">
                <a class="nav-link active"  href="#">Home</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Serie Tv</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Film</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Originali</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Aggiunti di recente</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">La mia lista</a>
              </li>
            </ul>
          </div>
        </div>
        <div class="d-flex">
          <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" v-model="userSearch" @keyup.enter="$emit('search',userSearch)"> 
          <button class="btn btn-outline-success" @click="$emit('search',userSearch)">Search</button>
        </div>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'indexHeader',
  data:function(){
    return{
      userSearch:""
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
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
