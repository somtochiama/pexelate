k<template>
    <div class="col-md-4 img-box" @click="$emit('photo-clicked')">
        <div class="card my-4">
            <img :src="src" :alt="alt">
        </div>
    </div>
</template>

<script>
export default {
    name: "HomepagePhoto",
    props: ['src', 'alt'],
    methods: {
        viewPhoto(id) {
            $this.router.push('/')
        }
    }
}
</script>

<style scoped>

    img {
        max-width: 100%;
        min-width: 100%;
        max-height: 100%;
        min-height: 100%;
    }

    .img-box{
        transition: all 0.3s
    }

    .img-box:hover {
       background-color: rgba(0, 0, 0, 0.3)
    }


</style>
