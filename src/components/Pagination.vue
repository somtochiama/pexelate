<template>
    <nav aria-label="Page navigation">
        <ul class="pagination pagination-lg justify-content-center">
            <li class="page-item" :class="{disabled: !prev}">
            <a class="page-link" href="#" aria-label="Previous" @click="$emit('search', prev)">
                <span aria-hidden="true">&laquo;</span>
                <span class="sr-only">Previous</span>
            </a>
            </li>
            <li class="page-item active disabled" :aria-current="page">
                <a class="page-link" href="#">{{page}}<span class="sr-only"></span></a>
            </li>
            <li class="page-item" :class="{disabled: !next}">
            <a class="page-link" href="#" aria-label="Next" @click="$emit('search', next)">
                <span aria-hidden="true">&raquo;</span>
                <span class="sr-only">Next</span>
            </a>
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
    name: "Pagination",
    props: ['next', 'prev', 'page'],
    data() {
        return {
            text: null,
        }
    },
    methods: {
        play() {
            console.log("A link was clicked")
        }
    },
}
</script>

<style>

</style>
