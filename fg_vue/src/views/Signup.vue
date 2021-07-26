<template>
  <div class="signup-page">
      <div class="columns">
          <div class="column is-4 is-offset-4">
              <h1 class="title">Sign Up</h1>
              <form @submit.prevent="submitForm">
                <div class="field">
                    <label>Username</label>
                    <div class="control">
                        <input type="text" class="input" v-model="username">
                    </div>
                </div>
                <div class="field">
                    <label>Password</label>
                    <div class="control">
                        <input type="password" class="input" v-model="password">
                    </div>
                </div>
                <div class="field">
                    <label>Confirm Password</label>
                    <div class="control">
                        <input type="password" class="input" v-model="password2">
                    </div>
                </div>

                <div class="notification is-danger" v-if="errors.length">
                    <p v-for="error in errors" :key="error">{{error}}</p>
                </div>
                <div class="field">
                    <div class="control">
                        <button class="button is-dark">Signup</button>
                    </div>
                </div>

                <hr>
                Or <router-link to="/login">Click here</router-link> to login!
              </form>
          </div>
      </div>
  </div>
</template>

<script>
import axios from 'axios';
import {toast} from 'bulma-toast';
export default {
    name:'Signup',
    data(){
        return {
            username: '',
            password: '',
            password2: '',
            errors: [],
        }
    },
    mounted(){
        document.title = 'Signup | Fitness Gear'
    },
    methods:{
        submitForm(){
            this.errors = []
            if(this.username === ''){
                this.errors.push('The username is required')
            }
            if(this.password === ''){
                this.errors.push('The password is too short')
            }
            if(this.password !== this.password2){
                this.errors.push("The passwords doesn't match")
            }

            if(!this.errors.length){
                const userData = {
                    username: this.username,
                    password: this.password
                }

                axios.post('/api/v1/users/', userData)
                .then(response => {
                    toast({
                        message: 'Account created, please login!',
                        type: 'is-success',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'top-center'
                    })
                    this.$router.push('/login')
                })
                .catch(e => {
                    if(e.response) {
                        for(const property in e.response.data) {
                            this.errors.push(`${property}: ${e.response.data[property]}`)
                        }
                        console.log(JSON.stringify(errors.response.data))
                    }
                    else if(e.message){
                        this.errors.push('Something went wrong. Please try again')
                        console.log(JSON.stringify(e))
                    }
                })
            }
        }
    },
}
</script>

<style>

</style>