<template>
    <form @submit.prevent>
        <div class="form-group row">
            <div class="col-3">
                <input type="text" placeholder="Kino nomi" class="form-control" :value="name" @:input="name = $event.target.value">
            </div>
            <div class="col-3">
                <input type="number" placeholder="Ko'rishlar soni" class="form-control" :value="viewers" @:input="viewers = $event.target.value">
            </div>
            <div class="col-3">
                <input type="submit" class="btn btn-dark" value="Qo'shish" @:click="addMovie">
            </div>
        </div>
    </form>
</template>
<script>
export default {
    data(){
        return{
            name: '',
            viewers: '',
        }
    },
    methods: {
        addMovie(){
            if(!this.name || !this.viewers) return
            const newMovie = {
                name: this.name,
                viewers: this.viewers,
                like: false,
                favourite: false,
            }
            this.$emit('createMovie', newMovie)
        }
    },
}
</script>
<style>
    
</style>