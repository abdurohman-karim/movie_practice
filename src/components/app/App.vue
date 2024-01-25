<template>
  <AppHeader />
  <div class="container mt-5">
    <div class="row">
      <div class="col-12">
        <div class="card">
          <div class="card-header">Filter</div>
          <div class="card-body">
            <SearchBar :updateTermHandler="updateTermHandler" />
          </div>
          <div class="card-footer">
            <AppFilter :updateFilterHandler="updateFilterHandler" :filterName="filter" />
          </div>
        </div>
      </div>
    </div>
    <div class="row mt-3">
      <div class="col-12">
        <div class="card">
          <div class="card-header">Movies</div>
          <div class="card-body" v-if="isLoaded">
            <div class="d-flex align-items-center justify-content-center">
              <Loader />
            </div>
          </div>
          <div class="card-body" v-else>
            <MovieList :movies="onFilterHandler(onSearchHandler(movies, term), filter)" @onToggle="onToggleHandler" @onDelete="onDeleteHandler" />
          </div>
          <div class="card-footer">
            <nav aria-label="pagination" v-if="this.movies">
            <ul class="pagination">
              <li class="page-item" v-if="this.currentPage > 1">
                <a class="page-link" href="#" aria-label="Previous" @click="prevPageHandler">
                  <span aria-hidden="true">&laquo;</span>
                </a>
              </li>
              <li class="page-item" v-for="pageNumber in totalPages" :key="pageNumber" :class="{active: this.page == pageNumber }" @click="updatePageHandler(pageNumber)"><a class="page-link" href="#">{{ pageNumber }}</a></li>
              
              <li class="page-item" v-if="this.currentPage < this.totalPages">
                <a class="page-link" href="#" aria-label="Next" @click="nextPageHandler">
                  <span aria-hidden="true">&raquo;</span>
                </a>
              </li>
            </ul>
          </nav>
          </div>
        </div>
      </div>
    </div>
    <div class="row mt-3">
      <div class="col-12">
        <div class="card">
          <div class="card-header">Kino qo'shish</div>
          <div class="card-body">
            <MovieCreate @createMovie="createMovie" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppHeader from "@/components/app-header/AppHeader.vue"
import AppFilter from "@/components/filters/AppFilter.vue"
import SearchBar from "@/components/search-bar/SearchBar.vue"
import MovieList from "@/components/movie-list/MovieList.vue"
import MovieCreate from "@/components/movie-create/MovieCreate.vue"
import Loader from "@/components/ui-components/Loader.vue"
import axios from 'axios'

export default{
  components: {
    AppHeader,
    AppFilter,
    SearchBar,
    MovieList,
    MovieCreate,
    Loader
  },
  data(){
        return{
            movies: [],
            term: '',
            filter: 'all',
            isLoaded: false,
            limit: 5,
            page: 1,
            totalPages: 0,
            currentPage: 1,
        }
    },
    methods: {
        async createMovie(item){
          try {
            const response = await axios.post('https://d7afa4aa6e3c65ec.mokky.dev/items', item)
            this.movies.push(response.data)
          } catch (error) {
            console.log(error.message);
          }
        },
        onToggleHandler({id, prop}){
          this.movies = this.movies.map(item => {
            if(item.id == id){
              return {...item, [prop]: !item[prop]}
            }
            return item
          })
        },
        async onDeleteHandler(id){
          try {
            const response = await axios.delete(`https://d7afa4aa6e3c65ec.mokky.dev/items/${id}`)
          } catch (error) {
            console.log(error.message);
          }
        },
        onSearchHandler(arr, term){
          if(term == 0){
            return arr
          }

          return arr.filter(c => c.name.toLowerCase().indexOf(term) > -1)
        },
        onFilterHandler(arr, filter){
          switch (filter) {
            case 'saved':
              return arr.filter(c => c.favourite)
            case 'popular':
              return arr.filter(c => c.viewers > 500)
            default:
              return arr
          }
        },
        updateTermHandler(term){
          this.term = term
        },
        updateFilterHandler(filter){
          this.filter = filter
        },
        async fetchMovies(){
          try {
            this.isLoaded = true
            const response = await axios.get('https://d7afa4aa6e3c65ec.mokky.dev/items', {
              params: {
                limit: this.limit,
                page: this.page
              }
            })
            this.totalPages = response.data.meta.total_pages
            this.currentPage = response.data.meta.current_page
            this.movies = response.data.items
    
          } catch (error) {
            console.log(error.message);
          } finally{
            this.isLoaded = false
          }
        },
        updatePageHandler(page){
          this.page = page
        },
        nextPageHandler(){
          if(this.page < this.totalPages){
            this.page += 1
          }
        },
        prevPageHandler(){
          if(this.page > 1){
            this.page -= 1
          }
        }
    },
    mounted() {
      this.fetchMovies()
    },
    watch: {
      page(){
        this.fetchMovies()
      }
    }
}
</script>

<style>

</style>