<template>
  <div class="home">
    <!-- <div class="container-fluid"> -->
      <main role="main">
        <section class="jumbotron jumbotron-fluid text-center main-banner">
          <div class="container">
            <h1 class="display-4">Pexelate</h1>
            <p class="lead">Quickly find pictures from <a href="https://www.pexels.com/">Pexels</a></p>
          </div>
        </section>
        <div class="d-flex justify-content-center gif-box" v-if="loading">
          <img src="../assets/loading.gif" alt="A loading Icon">
        </div>
        <div class="album py-5 bg-light">
          <div class="container">
            <div class="row">
              <homepage-photo
                v-for="photo in photos"
                :key="photo.id"
                :src="photo.src.landscape"
                :alt="photo.photographer + '\'s photo'"
                @photo-clicked="viewPhoto(photo.id)"
              />
            </div>
          </div>
        </div>
      </main>
      <footer>
        <div class="container">
          <pagination 
            :next="next_url"
            :prev="prev_url"
            :page="page"
            @search="searchPhoto"/>
        </div>
      </footer>
    <!-- </div> -->
    <!-- <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" /> -->
  </div>
</template>

<script>
import axios from "axios"
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import Pagination from "@/components/Pagination.vue";
import HomepagePhoto from "@/components/HomepagePhoto.vue";
import { constants } from 'crypto';

export default {
  name: "home",
  components: {
    Pagination,
    HomepagePhoto
  },
  data() {
    return {
      next_url: null,
      prev_url: null,
      page: 1,
      photos: null,
      loading: false
    }
  },
  methods: {
    searchPhoto(url ="https://api.pexels.com/v1/search", text = null) {
        this.loading = true
        this.photos = null;
        console.log("submit")
        let params
        if (!text) {
          params = null
        } else {
          url ="https://api.pexels.com/v1/search",
          params = {
              query: text,
              per_page: 21,
          }
        }
        axios({
            method: "get",
            url,
            params,
            headers: {
                Authorization: "563492ad6f917000010000017b64cd40db4646c5aef2269c6cd114d6"
            }
        })
        .then((response) => {
          console.log(response)
          const next_page = response.data.next_page
          const prev_page = response.data.prev_page
          this.loading = false
          this.photos = response.data.photos
          this.next_url = next_page ? next_page : null
          this.prev_url = prev_page ? prev_page : null
          this.page = response.data.page
        })
        .catch((err) => {
          this.loading = false
          console.log(err)
          console.log(err.response.data)
        })
    },
    viewPhoto(id) {
      this.$router.push({name: "photo", params: {id}})
    }
  },
  mounted() {
    let url, params
    console.log("mounted", this.$route)
    if (this.$route.name === "search") {
      console.log("search")
      url = "https://api.pexels.com/v1/search"
      this.searchPhoto(url, this.$route.query.plan)
      
    } else {
      let url = "https://api.pexels.com/v1/curated?per_page=21&page=1";
      this.searchPhoto(url)
    }
  },
  watch: {
    '$route' (to, from) {
      // react to route changes...
      console.log("watching")
      if (to.name === "search") {
        let url = "https://api.pexels.com/v1/search"
        this.searchPhoto(url, to.query.plan)
      } else {
        let url = "https://api.pexels.com/v1/curated?per_page=21&page=1";
        this.searchPhoto(url)
      }
    }
  }

};
</script>



