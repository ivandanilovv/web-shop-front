<template>
  <div>
    <div class="d-flex align-items-center justify-content-center">
      <table class="table table-striped table-hover table-responsive w-50">
        <thead>
        <tr>
          <th scope="col">
            Category Id
          </th>
          <th scope="col">
            Category Name
          </th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="category in categories.data">
          <th scope="row" class="align-middle">
            {{ category.id }}
          </th>
          <td class="align-middle fs-4 text-primary">
            {{ category.name }}
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "CategoriesTable",
  props: {
    categories: Array,
  }
}
</script>

<style scoped>

</style>
