<template>
  <div class="container">
    <custom-navbar active-page="products"/>
    <div class="row">
      <div class="col-5 d-flex align-items-start justify-content-start mt-5">
        <p class="mt-1">
          View products for:
        </p>
        <select class="form-control w-30 ms-3" id="selectCategory" v-model="key">
          <option v-for="category in categories.data" :value="category.name">
            {{ category.name }}
          </option>
        </select>
        <button class="btn btn-primary ms-3" @click="getCategoryId">
          Filter products
        </button>
      </div>
      <div class="col-7 d-flex align-items-center justify-content-center" v-if="searchCategory">
        <table class="table table-striped table-hover table-responsive">
          <thead>
          <tr>
            <th scope="col">#</th>
            <th scope="col">Category Name</th>
            <th scope="col">Category</th>
            <th scope="col">View product</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="product in products.data" v-if="product.category_id===searchCategoryId">
            <th scope="row" class="align-middle">{{ product.id }}</th>
            <td class="align-middle">{{ product.name }}</td>
            <td class="align-middle">{{ searchCategory }}</td>
            <td class="align-middle">
              <button class="btn btn-primary" @click="getProduct(product.id)">
                View product
              </button>
            </td>
          </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div class="d-flex justify-content-center align-items-center" v-if="product">
      <div class="card w-30 rounded-4">
        <img src="https://media.istockphoto.com/photos/mountain-landscape-picture-id517188688?k=20&m=517188688&s=612x612&w=0&h=i38qBm2P-6V4vZVEaMy_TaTEaoCMkYhvLCysE7yJQ5Q="
             class="img-fluid rounded-top" alt="...">
        <div class="card-body">
          <h5 class="card-title">
            {{product.data.name}}
          </h5>
          <p class="card-text">
            {{product.data.description}}
          </p>
          <p>
            Price: <span class="text-primary">{{product.data.price}}&euro;</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CustomNavbar from "@/components/CustomNavbar";

export default {
  name: "products",
  components: {
    CustomNavbar
  },
  props: {
    key: String,
  },
  data() {
    return {
      categories: '',
      products: '',
      searchCategory: '',
      searchCategoryId: null,
      product: null,
    }
  },
  created() {
    this.getCategories();
    this.getProducts();
  },
  methods: {
    async getCategories() {
      this.categories = await this.$axios.$get('http://127.0.0.1:8000/api/categories')
    },
    async getProducts() {
      this.products = await this.$axios.$get('http://127.0.0.1:8000/api/products')
    },
    async getProduct(id) {
      this.product = (await this.$axios.$get('http://127.0.0.1:8000/api/products/' + id))
    },
    getCategoryId() {
      this.searchCategory = this.key;
      for (let i = 0; i < this.categories.data.length; i++) {
        if (this.searchCategory === this.categories.data[i].name) {
          this.searchCategoryId = this.categories.data[i].id;
        }
      }
    },
  }
}
</script>

<style scoped>
.w-30 {
  width: 30% !important;
}
</style>
