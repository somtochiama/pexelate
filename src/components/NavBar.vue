<template>
    <nav class="navbar navbar-expand-lg navbar-light bg-light sticky-top justify-content-between">
        <router-link class="navbar-brand" to="/">Pexelate</router-link>
        <form class="form-inline my-2 my-lg-0 ">
            <!-- <input class="form-control mr-sm-2" type="search" placeholder="Search for photos..." aria-label="Search" v-model="query">
            <button class="btn btn-outline-success my-2 my-sm-0" type="submit" >Submit</button> -->
            <div class="input-group">
                <input type="text" class="form-control" v-model="query" v-on:keydown.enter.prevent="emitSearch()" placeholder="Search for photos.." aria-label="Search for button" aria-describedby="button-addon2">
                <div class="input-group-append">
                    <button class="btn btn-outline-success" type="button" id="button-addon2"  @click="emitSearch()">Submit</button>
                </div>
            </div>
        </form>
    </nav>
</template>

<script>
// import axios from "axios";


export default {
    name: "NavBar",
    data() {
        return {
            query: null,
        }
    },
    methods: {
        emitSearch() {
            this.$emit('search', this.query)
            this.query = null
        }
    }
}
</script>

<style>

</style>
