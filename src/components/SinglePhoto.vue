<template>
    <div class="photo">
        <div class="d-flex justify-content-center gif-box" v-if="loading">
          <img src="../assets/loading.gif" alt="A loading Icon">
        </div>
        <div class="d-md-flex flex-md-equal w-100 px-md-3 mt-5" v-if="data">
            <div class="mr-md-3 pt-3 px-3 pt-md-5 px-md-5 text-center text-white overflow-hidden img-box">
                <img 
                   class="single-photo" :src="data.src.landscape" :alt="data.photographer + '\'s photo'" 
                />
            </div>
            <div class="mr-md-3 pt-3 px-3 pt-md-5 px-md-5 mx-auto mt-lg-6 text-center overflow-hidden photo-info">
                <h3>Photographer: {{data.photographer}}</h3>
                <p>
                  <a href="http://" target="_blank">Link to his other works</a>  
                </p>
                <button class="btn btn-success" @click="downloadPhoto()">
                    Download Picture
                </button>
                <!-- <a ref="downloadLink" :href="data.src.medium" >Download</a>  -->

            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios"
import { type } from 'os';

export default {
    name: "SinglePhoto",
    // props: ['src', 'alt'],
    data() {
        return {
            data: null,
            loading: false
        }
    },
    methods: {
        downloadPhoto() {
            axios({
                url: this.data.src.medium,
                method: 'get',
                responseType: 'arraybuffer', 
                })
                .then((response) => {
                    const url = window.URL.createObjectURL(new Blob([response.data]));
                    const link = document.createElement('a')
                    link.href = url
                    let str = response.headers["content-type"]
                    console.log(str)
                    let fileName = str.replace("/", ".")
                    link.setAttribute('download', fileName) //or any other extension
                    document.body.appendChild(link)
                    link.click()
                    link.remove()

                })
                .catch((err) => {
                    console.log(err)
                    console.log(err.response.data)
                })
        }
    },
    mounted() {
        this.loading = true
        axios({
            method: "get",
            url: `https://api.pexels.com/v1/photos/${this.$route.params.id}`,
            headers: {
            Authorization: "563492ad6f917000010000017b64cd40db4646c5aef2269c6cd114d6"
            }
        })
        .then((response) => {
            (response)
            this.loading = false
            this.data = response.data
        })
    },
}
</script>

<style scoped>
    .single-photo {
        max-width: 100%;
        min-width: 100%;
        max-height: 100%;
        min-height: 100%;
    }
</style>
