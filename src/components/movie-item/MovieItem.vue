<template>
    <li class="list-group-item d-flex justify-content-between align-items-center" :class="{ 'favourite': movie.favourite }">
        <span><b>#{{movie.id}} </b> {{movie.name}}</span> 
        <input type="number" class="form-control w-25" v-bind:value="movie.viewers">
        <div class="btn-group">
            <a href="#" class="btn btn-warning" @click="$emit('onToggle', {id: movie.id, prop: 'favourite'})"><i class="fas fa-cookie"></i></a>
            <a href="#" class="btn btn-danger" @click="$emit('onDelete', movie.id)"><i class="fas fa-trash"></i></a>
            <a href="#" class="btn" @click="$emit('onToggle', {id: movie.id, prop: 'like'})" :class="{ 'like': movie.like }"><i class="fas fa-star"></i></a>
        </div>
    </li>
</template>

<script>
export default {
    props:{
        movie: {
            type: Object,
            required: true,
        },
    },
    methods: {
        onLike(){
            this.$emit('onLike', this.movie.id)
        },
        onFavourite(){
            this.$emit('onFavourite', this.movie.id)
        }
    }
}
</script>
<style>
    .like{
        background-color: yellow;
    }
    .like:hover{
        background-color: yellow;
    }
    .favourite{
        background-color: rgba(113, 255, 177, 0.808);
    }
</style>