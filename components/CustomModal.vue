<template>
  <div class="modal-body">
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
        <span class="visually-hidden">
          Previous
        </span>
      </button>
      <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleControls"
              data-bs-slide="next">
        <span class="carousel-control-next-icon" aria-hidden="true"></span>
        <span class="visually-hidden">
          Next
        </span>
      </button>
    </div>
    <h2 class="mt-3 fw-bold text-primary">
      {{ product.data.name }}
    </h2>
    <p class="fs-5">
      {{ product.data.description }}
    </p>
    <p class="fs-4">
      Price:
      <span class="text-primary fw-bold">
        {{ product.data.price }}&euro;
      </span>
    </p>
  </div>
</template>

<script>
export default {
  name: "CustomModal",
  props: {
    allImages: Array,
    product: Object,
  }
}
</script>

<style scoped>

</style>
