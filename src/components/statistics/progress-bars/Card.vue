<template>
  <div class="card" style="width: 100%; height:100%;">
    <div class="card-body">
        <router-link tag="a" :to="{name: 'product', params: {id: product._id}}">
            <img class="card-img-top" :src="product.image" :alt="product.name">
        </router-link>
        <h5 class="card-title text-info">{{ product.name }}</h5>
        <p><span class="font-weight-bold">Category</span>: {{ product.category }}</p>
        <p class="card-text">{{ product.description.substring(0, 50) }}</p>
        <p class="card-text text-danger">${{ product.price }}</p>
        <b-btn variant="success" @click.stop="editProduct(product._id)">Edit</b-btn>
        <b-btn variant="danger" @click.stop="deleteProduct(product._id)">Delete</b-btn>

    </div>
</div>
</template>

<script>
import axios from 'axios'
export default {
  props: ['product'],
  data () {
    return {
    product: {}
    }
  },
methods: {
    deleteProduct(productid) {
    axios.delete('http://localhost:3000/product/' + product._id
    )},

    editProduct (productid) {
    this.$router.push({
        name: 'EditProduct',
        params: { id: productid }
    })
}
}
}
</script>

<style scoped>
.card-title {
        cursor: pointer;
    }
</style>