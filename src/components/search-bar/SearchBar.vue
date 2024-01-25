<template>
    <form action="#">
        <div class="form-group">
            <input type="search" class="form-control" placeholder="Qidirish" :value="term" @input="changeHandler">
            <button class="btn btn-primary mt-4">Qidirish</button>
        </div>
    </form>
</template>
<script>
export default {
    props: {
        updateTermHandler: {
            type: Function,
            required: true,
        }
    },
    data(){
        return{
            term: '',
        }
    },
    methods: {
        changeHandler(e){
            this.term = e.target.value
            this.updateTermHandler(this.term)
        }
    },
}
</script>
<style scoped>
    
</style>