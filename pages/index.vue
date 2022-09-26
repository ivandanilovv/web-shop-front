<template>
  <div class="">
    <button @click="fetchSomething">click</button>
    {{ip}}
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      ip: '',
    }
  },
  methods: {
    async fetchSomething() {
      this.ip = await this.$axios.$get('http://127.0.0.1:8000/api/products')
    }
  }
}
</script>
