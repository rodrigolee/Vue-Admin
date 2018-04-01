<template>
  <div class="container">
    <div class="row">
      <div class="col-md-4"></div>
      <!-- <div class="col-md-8">
        <img :src="product.image" :alt="product.name" class="img-fluid">
        <h5>{{ product.name }}</h5>
        <p><span class="font-weight-bold">Category</span>: {{ product.category }}</p>
        <p class="text-danger font-weight-bold">${{product.price}}</p>
        <p>
          {{ product.description }}
        </p>
      </div> -->
    </div>
    <b-row>
    <b-col cols="12">
      <h2>
        Edit Product
        <router-link :to="{ name: 'progress-bars', params: { id: product._id } }">Show Product</router-link>
      </h2>
      <b-form @submit="onSubmit">
        <b-form-group id="fieldsetHorizontal"
                  horizontal
                  :label-cols="4"
                  breakpoint="md"
                  label="Enter Name">
          <b-form-input id="name" v-model.trim="product.name"></b-form-input>
        </b-form-group>
        <b-form-group id="fieldsetHorizontal"
                  horizontal
                  :label-cols="4"
                  breakpoint="md"
                  label="Enter Title">
          <b-form-input id="price"  v-model.trim="product.price"></b-form-input>
        </b-form-group>
        <b-form-group id="fieldsetHorizontal"
                  horizontal
                  :label-cols="4"
                  breakpoint="md"
                  label="Enter Description">
            <b-form-textarea id="description"
                       v-model="product.description"
                       placeholder="Enter something"
                       :rows="2"
                       :max-rows="6">{{product.description}}</b-form-textarea>
        </b-form-group>
        <b-button type="submit" variant="primary">Update</b-button>
      </b-form>
    </b-col>
  </b-row>
  </div>
</template>

<script>
import axios from 'axios'
import Api from '../../../../config/Api'
export default {
  name: 'EditProduct',
  props: ['id'],

  data () {
    return {
      product: {}
    }
  },
  created() {
       Api().get(`/products/${this.id}`)
              .then(response => {
                this.product = response.data
              });
  },
  methods: {
    onSubmit (evt) {
      evt.preventDefault()
      axios.put(`http://localhost:3000/products/` + this.$route.params.id, this.product)
      .then(response => {
        this.$router.push({
          params: { id: this.$route.params.id }
        })
      })
      .catch(e => {
        this.errors.push(e)
      })
    }
    
  }
}
</script>

<style>
</style>