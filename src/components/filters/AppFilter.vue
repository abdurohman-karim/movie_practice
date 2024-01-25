<template>
    <form class="btn-group">
        <button v-for="btn in filterButtons" :key="btn.name" class="btn" :class="[filterName == btn.name ? 'btn-dark' : 'btn-outline-dark']" @click="filterHandler(btn.name)">
            {{ btn.title }}
        </button>
    </form>
</template>
<script>
export default {
    props: {
        updateFilterHandler:{
            type: Function,
            required: true
        },
        filterName:{
            type: String,
            required: true,
        }
    },
    data() {
        return {
            filter: 'all',
            filterButtons: [
                {
                    title: "Barchasi",
                    name: 'all',
                },
                {
                    title: "Saqlangan kitoblar",
                    name: 'saved',
                },
                {
                    title: "Eng ko'p ko'rilganlar",
                    name: 'popular',
                },
            ]
        }
    },
    methods: {
        filterHandler(filter){
            this.filter = filter
            this.updateFilterHandler(this.filter)
        }
    },
}
</script>
<style>
    
</style>