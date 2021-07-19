<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
      <div class="hero-body has-text-centered">
        <p class="title mb-6">
          Welcome to FitnessGear
        </p>
        <p class="subtitle">
          The best fitness online store
        </p>
      </div>
    </section>
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">Latest products</h2>
      </div>
      <ProductBox v-for="product in latestProducts" :key="product.id" :product="product" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'
export default {
  name: 'Home',
  data(){
    return {
      latestProducts: []
    }
  },
  components: {
    ProductBox
  },
  mounted(){
    this.getLatestProducts()
     document.title = 'Home | FitnessGear'
  },
  methods: {
    async getLatestProducts(){
      this.$store.commit('setIsLoading', true)
      await axios.get('/api/v1/latest-products/').then(response =>{
        this.latestProducts = response.data
        console.log(response.data[0].get_thumbnail)
      })
      .catch(e =>{
        console.log(e)
      })
       this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

