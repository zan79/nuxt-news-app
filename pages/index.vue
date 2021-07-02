<template>
  <div>
    <div>
      <b-navbar variant="dark" type="dark" class="px-5">
        <b-navbar-brand tag="h1" class="mb-0">My News App</b-navbar-brand>

        <b-navbar-nav class="">
          <b-nav-item href="#">Headlines</b-nav-item>
        </b-navbar-nav>

        <b-navbar-nav class="mx-5">
          <b-nav-form @submit.prevent="onSearch">
            <b-form-input id="searchfield" class="mr-sm-2" v-model="search" placeholder="Search"></b-form-input>
            <b-button variant="primary" class="my-2 my-sm-0" type="submit">Search</b-button>
          </b-nav-form>
        </b-navbar-nav>
        
      </b-navbar>
    </div>

    <div class="container d-flex justify-content-between flex-wrap pt-5">

      <!-- <div class="card article mb-4" v-for="article, index in results.articles" :key="index">
        <img :src="article.urlToImage" alt="img here" class="card-img-top">
        <div class="card-header">
          <h5 class="card-title">
            <a :href="article.url" target="_blank">{{article.title}}</a>
          </h5>
          <div>{{article.author}}</div>
        </div>
        <div class="card-body">
          {{article.content}}
        </div>
      </div> -->

      <div class="card mb-3" v-for="article, index in results.articles" :key="index">
        <img :src="article.urlToImage" alt="img here" class="card-img-top">
        <div class="card-header">
          <div>{{article.author}}</div>
        </div>
        <div class="card-body">
          <a :href="article.url" target="_blank">{{article.title}}</a>
        </div>
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      results: {},
      search: ''
    }
  },
  created(){
    this.getHeadlines()
  },
  methods:{
    async getHeadlines(){
      await this.$axios.get('https://newsapi.org/v2/top-headlines?sources=techcrunch&apiKey=796a4a2cc8074275b10cb45887a8fc7f')
      .then ( (res)=> {
        if(res.status==200) {
          this.results = res.data
        }
      })
    },
    async onSearch(){
      if (!this.search) return
      await this.$axios.get('https://newsapi.org/v2/everything?q=' + this.search + '&from=2021-05-23&sortBy=publishedAt&apiKey=796a4a2cc8074275b10cb45887a8fc7f')
      .then ( (res)=> {
        if(res.status==200) {
          this.results = res.data
        }
      })
    }
  }
}
</script>

<style >
body{
  background-color: #242424;
}
.card{
  width: 32.5%;
  background-color: #2b2b2b;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.12) 0px 1px 3px, rgba(0, 0, 0, 0.24) 0px 1px 2px;
}
.card-header{
  color: #fff;
}
.card-img-top {
    width: 100%;
    height: 15vw;
    border-radius: 10px 10px 0px 0px;
    object-fit: cover;
}
</style>
