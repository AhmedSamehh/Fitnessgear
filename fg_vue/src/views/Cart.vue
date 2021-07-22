<template>
  <div class="cart-page">
      <div class="is-multiline columns">
          <div class="column is-12">
              <h1 class="title">Cart</h1>
          </div>
          <div class="column is-12 box">
              <table class="table is-fullwidth" v-if="cartTotalLength">
                  <thead>
                      <tr>
                        <th>Product</th>
                        <th>Price</th>
                        <th>Quantity</th>
                        <th>Total</th>
                        <th></th>  
                      </tr>
                  </thead>
                  <tbody>
                      <CartItem 
                      v-for="item in cart.items"
                      :key="item.product.id"
                      :initialItem="item" />
                  </tbody>
              </table>
              <p v-else>You don't have any items in your cart..</p>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios'
import CartItem from '@/components/CartItem'
export default {
    name:'Cart',
    data(){
        return {
            cart: {
                items : []
            }
        }
    },
    components:{
        CartItem
    },
    mounted(){
        this.cart = this.$store.state.cart
    },
    computed:{
        cartTotalLength() {
            return this.cart.items.reduce((acc, curVal) => {
                return acc += curVal.quantity
            }, 0)
        }
    }
}
</script>

<style>

</style>