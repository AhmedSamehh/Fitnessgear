<template>
  <div class="search-page">
      <div class="is-multiline columns">
          <div class="column is-12">
              <h1 class="title">Search</h1>
              <h2 class="is-size-5 has-text-grey">Search term: "{{query}}"</h2>
          </div>
          <ProductBox v-for="product in products" 
          :key="product.id" :product="product" />
      </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'
export default {
    name:"Search",
    data() {
        return {
            products: [],
            query: ""
        }
    },
    components:{
        ProductBox
    },
    mounted() {
        document.title = "Search | FitnessGear"
        let url = window.location.search.substring(1)
        let params = new URLSearchParams(url)

        if(params.get('query')) {
            this.query = params.get('query')
            this.performSearch()
        }
    },
    methods:{
        async performSearch(){
            this.$store.commit("setIsLoading", true)

            axios.post('api/v1/products/search', {'query': this.query})
            .then(response => {
                this.products = response.data
            })
            .catch(e => {
                console.log(e)
            })

            this.$store.commit("setIsLoading", false)
        }
    }
}
</script>

<style>

</style>