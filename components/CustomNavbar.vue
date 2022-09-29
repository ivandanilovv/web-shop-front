<template>
  <div>
    <nav class="navbar navbar-expand navbar-light bg-light">
      <div class="container-fluid">
        <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
          <div class="navbar-nav">
            <nuxt-link to="/" class="nav-link fs-4 text-primary" :class="{'active' : activePage === 'categories'}">
              Categories
            </nuxt-link>
            <nuxt-link to="/products" class="nav-link fs-4 text-primary" :class="{'active' : activePage === 'products'}">
              Products
            </nuxt-link>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "CustomNavbar",
  props: {
    activePage: String,
  }
}
</script>

<style scoped>
.active {
  font-weight: 700;
}
</style>
