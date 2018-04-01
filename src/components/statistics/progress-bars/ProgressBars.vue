<template>
  <div class="progress-bars">
    <div class="row">
      <div class="col-md-12">
        <vuestic-widget headerText="Progress Bars">
          <div class="row">
            <div class="col-sm-4 col-12">
              {{'progressBars.basic' | translate}}
              <div class="pb-container">
                <vuestic-progress-bar ref="hBasic">
                </vuestic-progress-bar>
              </div>
            </div>
            <div class="col-sm-4 col-12">
              {{'progressBars.thin' | translate}}
              <div class="pb-container">
                <vuestic-progress-bar size="thin" ref="hThin">
                </vuestic-progress-bar>
              </div>
            </div>
            <div class="col-sm-4 col-12">
            </div>            
          </div>
        </vuestic-widget>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <vuestic-widget headerText="Add Product">
          <form @submit="addProduct">
            <select class ="categories" v-model.trim="product.category">
            <option v-for="category in categories" :key="category._id">
              {{ category.title }}
            </option>
          </select>
          <input type="text" class="product-input" placeholder="Name" id="name" v-model.trim="product.name">
          <input type="integer" class="product-input" placeholder="Price" id="price" v-model.trim="product.price">
          <input class="product-input"  placeholder="Description" id="description" v-model.trim="product.description">
          <button type="submit" name="button">Submit</button>
          </form>
          
        </vuestic-widget>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <h2>Top Products</h2>
        <div class="products">
          <Cards :products="products" />
              <p class="text-center mb-0">{{currentPage+1 }} / {{ pages }}</p>
            <ul class="pagination justify-content-center">
                <li class="page-item" :class="{disabled: prevUrl === ''}">
                    <button class="page-link" @click="checkPage(prevUrl)">Previous</button>
                </li>
                <li class="page-item" :class="{disabled: nextUrl === ''}">
                    <button class="page-link" @click="checkPage(nextUrl)">Next</button>
                </li>
            </ul>
       </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Api from '../../../../config/Api'
import Cards from './Cards'
export default {
  props: [],
  components: {
    Cards
  },
  name: 'progress-bars',
  mounted () {
    let delay = 0
    for (let ref in this.$refs) {
      this.$refs[ref].$data.valueAnimationInterval = this.valueAnimationInterval
      let timeout = setTimeout(() => {
        if (!this.$refs[ref]) {
          clearTimeout(timeout)
          return
        }
        this.$refs[ref].$data.value = 100
      }, delay)
      delay += this.valueAnimationInterval
    }
  },
  data () {
    return {
      valueAnimationInterval: 3000,
      products: [],
      product: {},
      categories: [],
      currentPage: '',
      pages: '',
      prevUrl: '',
      nextUrl: ''
      }
    },
    created() {
      Api().get('/products')
        .then(response => {
            this.products = response.data.products;
            this.currentPage = response.data.currentPage;
            this.pages = response.data.pages;
            this.nextUrl = response.data.nextUrl;
            this.prevUrl = response.data.prevUrl;
            });
      Api().get('/categories')
        .then(response => {
            this.categories = response.data;
        });
        // console.log(this.products);
        console.log(this.categories);
    },
    methods: {
    addProduct (evt) {
      evt.preventDefault()
      axios.post('http://localhost:3000/products', this.product)
      .then(response => {
        this.$router.push({
          params: { id: response.data._id }
        })
      })
      .catch(e => {
        this.errors.push(e)
      })
    },
    editProduct (productid) {
      this.$router.push({
        name: 'EditProduct',
        params: { id: productid}
      })
    },
    checkPage(url) {
      Api().get(url)
        .then(response => {
					this.products = response.data.products;
					this.currentPage = response.data.currentPage;
					this.pages = response.data.pages;
					this.nextUrl = response.data.nextUrl;
					this.prevUrl = response.data.prevUrl;
        })
      }
    }
  }

</script>

<style lang="scss">
  @import "../../../sass/variables";
  @import "../../../../node_modules/bootstrap/scss/functions";
  @import "../../../../node_modules/bootstrap/scss/variables";
  @import "../../../sass/mixins";

  .progress-bars {
    .vuestic-progress-bar {
      .progress.thick {
        margin-top: calc(#{$progress-bar-width-thick} / 2 - .125rem);
      }
      .progress.thin {
        margin-top: .125rem;
      }
    }
    .product-input{
      margin-left:2px;
    }


  }
</style>
