<template>
  <div class="container">
    <custom-navbar active-page="products"/>
    <div class="row pb-5">
      <div class="col-5">
        <div class="d-flex flex-row my-3">
          <p class="m-0 ms-3">
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

        <div class="d-flex justify-content-center align-items-center" v-if="product">
          <div class="card rounded-2 w-100 shadow">
            <div id="carouselExampleControls" class="carousel slide" data-bs-ride="carousel">
              <div class="carousel-inner">
                <div class="carousel-item ratio ratio-16x9" v-for="(item, i) in allImages" :class="{'active' : i===0}"
                     v-if="i!==allImages.length-1">
                  <img :src="'http://127.0.0.1:5173/public/images/' + item"
                       class="rounded-top" alt="Image">
                </div>
              </div>
              <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleControls"
                      data-bs-slide="prev">
                <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Previous</span>
              </button>
              <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleControls"
                      data-bs-slide="next">
                <span class="carousel-control-next-icon" aria-hidden="true"></span>
                <span class="visually-hidden">Next</span>
              </button>
            </div>
            <div class="card-body">
              <h5 class="card-title">
                {{ product.data.name }}
              </h5>
              <p class="card-text">
                {{ product.data.description }}
              </p>
              <p>
                Price: <span class="text-primary">{{ product.data.price }}&euro;</span>
              </p>
              <button class="btn btn-primary" @click="addToFavourites(product.data)">
                Add to favourites
              </button>
            </div>
          </div>
        </div>
      </div>
      <div class="col-7">
        <div class="d-flex flex-row justify-content-end mt-3">
          <favourites :favourite-products="favourites"/>
        </div>
        <div class="d-flex flex-column align-items-center" v-if="searchCategory">
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
                <button class="btn btn-primary" @click="getProduct(product.id), getImages(product.image)">
                  View product
                </button>
              </td>
            </tr>
            </tbody>
          </table>
          <div>
            <button type="button" :disabled="!products.links.prev" @click="getPage(products.links.prev)">
              Previous
            </button>
            <button type="button" :disabled="!products.links.next" @click="getPage(products.links.next)">
              Next
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CustomNavbar from "@/components/CustomNavbar";
import Favourites from "@/components/Favourites";

export default {
  name: "products",
  components: {
    Favourites,
    CustomNavbar
  },
  data() {
    return {
      key: '',
      page: 0,
      products: '',
      product: null,
      categories: '',
      allImages: null,
      searchCategory: '',
      searchCategoryId: null,
      favourites: [],
    }
  },
  created() {
    this.getProducts();
    this.getCategories();
  },
  mounted() {
    this.favourites = JSON.parse(localStorage.getItem('favourites') || "[]");
  },
  methods: {
    async getPage(link) {
      if (link !== null)
        this.products = await this.$axios.$get(link);
    },
    async getCategories() {
      this.categories = await this.$axios.$get('http://127.0.0.1:8000/api/categories')
    },
    async getProducts() {
      this.products = await this.$axios.$get('http://127.0.0.1:8000/api/products?page=1')
    },
    async getProduct(id) {
      this.product = (await this.$axios.$get('http://127.0.0.1:8000/api/products/' + id))
    },
    getCategoryId() {
      this.product = null;
      this.searchCategory = this.key;
      for (let i = 0; i < this.categories.data.length; i++) {
        if (this.searchCategory === this.categories.data[i].name) {
          this.searchCategoryId = this.categories.data[i].id;
        }
      }
    },
    getImages(item) {
      this.allImages = item.split(", ");
    },
    addToFavourites(product) {
      let exists = false;
      for(const item of this.favourites) {
        if (item.id === product.id) {
          exists = true;
          break;
        }
      }
      if (!exists) {
        this.favourites.push(product);
      }
    }
  },
  watch: {
    favourites: {
      handler(newValue) {
        localStorage.setItem('favourites', JSON.stringify(newValue));
      }, deep: true,
    }
  }
}
</script>

<style scoped>
.w-30 {
  width: 30% !important;
}
</style>
