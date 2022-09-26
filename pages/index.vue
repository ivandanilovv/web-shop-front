<template>
  <div>
    <button @click="fetchSomething">click</button>
    {{ip.data}}
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      ip: null,
    }
  },
  methods: {
    async fetchSomething() {
      this.ip = await this.$axios.$get('http://127.0.0.1:8000/api/products')
    }
  }
}
</script>
