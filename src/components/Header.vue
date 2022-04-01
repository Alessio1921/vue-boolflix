<template>
  <nav class="navbar navbar-light bg-light">
    <div class="container-fluid">
      <a class="navbar-brand">Netflix</a>
      <div class="d-flex">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" v-model="userSearch"> <!-- @keyup="$emit('search',userSearch)"-->
        <button class="btn btn-outline-success" @click="$emit('search',userSearch)">Search</button>
      </div>
    </div>
  </nav>
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
</style>
