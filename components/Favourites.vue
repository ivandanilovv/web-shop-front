<template>
  <div>
    <button class="btn btn-primary" type="button" data-bs-toggle="offcanvas" data-bs-target="#offcanvasRight"
            aria-controls="offcanvasRight">
      Favourites
    </button>
    <div class="offcanvas offcanvas-end" tabindex="-1" id="offcanvasRight" aria-labelledby="offcanvasRightLabel">
      <div class="offcanvas-header">
        <h5 id="offcanvasRightLabel">
          Favourites
        </h5>
        <button type="button" class="btn-close text-reset" data-bs-dismiss="offcanvas"
                aria-label="Close"></button>
      </div>
      <div class="offcanvas-body">
        <div v-if="favouriteProducts.length===0">
          You don't have favourite products!
        </div>
        <div class="card mb-3" v-for="(product, i) in favouriteProducts" :key="i">
          <div class="row">
            <div class="col-md-4">
              <img
                :src="'http://127.0.0.1:5173/public/images/' + product.image.substring(0, product.image.indexOf(','))"
                class="img-fluid rounded-start" alt="Image">
            </div>
            <div class="col">
              <div class="card-body">
                <div class="card-title fw-bold">
                  {{ product.name }}
                </div>
                <div class="card-text">
                  Price:
                  <span class="text-primary">
                    {{ product.price }}&euro;
                  </span>
                </div>
                <div class="d-grid">
                  <button class="btn btn-outline-danger w-50 mt-3" @click="removeFromFavourites(product)">
                    Remove
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Favourites",
  props: {
    favouriteProducts: Array,
  },
  methods: {
    removeFromFavourites(product) {
      const favourites = this.favouriteProducts;
      const index = favourites.findIndex(item => item.id === product.id);

      favourites.splice(index, 1);
      this.$emit('update:favouriteProducts', favourites);
    }
  }
}
</script>

<style scoped>

</style>
