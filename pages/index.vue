<template>
  <div class="container">
    <custom-navbar active-page="categories"/>
    <categories-table :categories="categories"/>
<!--    <div>-->
<!--      View products for:-->
<!--      <select class="form-control" id="selectCategory">-->
<!--        <option v-for="category in categories.data" :value="category.name">-->
<!--          {{ category.name }}-->
<!--        </option>-->
<!--      </select>-->
<!--      <button class="btn btn-primary" @click="getCategoryId()">Get</button>-->
<!--    </div>-->
  </div>
</template>

<script>
import CustomNavbar from "~/components/CustomNavbar";
import CategoriesTable from "@/components/CategoriesTable";

export default {
  name: 'IndexPage',
  components: {CategoriesTable, CustomNavbar},
  data() {
    return {
      categories: '',
      products: '',
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
  }
}
</script>
