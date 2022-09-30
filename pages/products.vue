<template>
  <div class="container">
    <custom-navbar active-page="products"/>
    <div v-if="product" class="modal fade" id="productModal" tabindex="-1"
         aria-labelledby="productModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="productModalLabel">
              {{ product.data.name }}
            </h5>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          </div>
          <custom-modal :all-images="allImages" :product="product"/>
          <div class="modal-footer">
            <button type="button" class="btn btn-primary"
                    @click="addToFavourites(product.data)">
              Add to favourites
            </button>
            <button type="button" class="btn btn-dark" data-bs-dismiss="modal">
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="row d-flex flex-row align-items-center justify-content-center my-3">
      <div class="col-12 col-md-6 mb-3 mb-md-0
      d-flex flex-row align-items-center justify-content-center justify-content-md-end">
        <p class="m-0 text-primary fs-5">
          View products for:
        </p>
        <select class="form-control w-30 ms-3 text-secondary" id="selectCategory" v-model="key">
          <option v-for="category in categories.data" :value="category.name" class="text-secondary">
            {{ category.name }}
          </option>
        </select>
      </div>
      <div class="col-12 col-md-6 d-flex flex-row align-items-center justify-content-center justify-content-md-start">
        <button class="btn btn-primary ms-sm-3 me-2 me-sm-4" @click="getCategoryId">
          Filter products
        </button>
        <favourites :favourite-products="favourites"/>
      </div>
    </div>
    <div class="d-flex flex-column align-items-center">
      <table class="table table-striped table-hover table-responsive w-75">
        <thead>
        <tr>
          <th scope="col">
            #
          </th>
          <th scope="col">
            Category Name
          </th>
          <th scope="col">
            Category
          </th>
          <th scope="col">
            View product
          </th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="product in products.data">
          <th scope="row" class="align-middle">
            {{ product.id }}
          </th>
          <td class="align-middle text-primary fs-5">
            {{ product.name }}
          </td>
          <td class="align-middle fs-5 text-secondary">
            {{ searchCategory }}
          </td>
          <td class="align-middle">
            <button class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#productModal"
                    @click="getProduct(product.id), getImages(product.image)">
              View product
            </button>
          </td>
        </tr>
        </tbody>
      </table>
      <div v-if="products.links">
        <button type="button" class="btn btn-primary" :disabled="!products.links.prev"
                @click="getPage(products.links.prev)">
          Previous
        </button>
        <button type="button" class="btn btn-primary" :disabled="!products.links.next"
                @click="getPage(products.links.next)">
          Next
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import Favourites from "@/components/Favourites";
import CustomNavbar from "@/components/CustomNavbar";
import CustomModal from "@/components/CustomModal";

export default {
  name: "products",
  components: {
    CustomModal,
    Favourites,
    CustomNavbar
  },
  data() {
    return {
      key: '',
      page: 0,
      products: '',
      product: null,
      favourites: [],
      categories: '',
      allImages: null,
      searchCategory: '',
      searchCategoryId: null,
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
      this.products = await this.$axios.$get('http://127.0.0.1:8000/api/products')
    },
    async getProduct(id) {
      this.product = (await this.$axios.$get('http://127.0.0.1:8000/api/products/' + id))
    },
    async getCategoryId() {
      this.searchCategory = this.key;
      for (let i = 0; i < this.categories.data.length; i++) {
        if (this.searchCategory === this.categories.data[i].name) {
          this.searchCategoryId = this.categories.data[i].id;
        }
      }
      this.products = await this.$axios.$get('http://127.0.0.1:8000/api/products?category_id=' + this.searchCategoryId);
    },
    getImages(item) {
      this.allImages = item.split(", ");
    },
    addToFavourites(product) {
      let exists = false;
      for (const item of this.favourites) {
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
@media(min-width: 768px) {
  .w-75 {
    width: 75%;
  }
}
</style>
