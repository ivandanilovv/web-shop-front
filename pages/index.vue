<template>
  <div class="container">
    <custom-navbar active-page="categories"/>
    <categories-table :categories="categories"/>
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
