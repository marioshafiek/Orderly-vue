<template>
  <div v-if="loadingStatus" class="flex justify-center">
    <div
      class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 p-4"
      :class="{ hidden: isMobile && isCartOpen }"
    >
      <ProductCard v-for="(product, index) in allProducts" :key="index" :product="product" />
    </div>
  </div>
  <div v-else>
    <div class="flex items-center justify-center min-h-screen">
      <div class="animate-spin rounded-full h-16 w-16 border-t-4 border-b-4 border-black"></div>
    </div>
  </div>
</template>

<script>
import ProductCard from './ProductCard.vue'

export default {
  components: {
    ProductCard
  },
  data() {
    return {
      isMobile: false // Track if the view is mobile
    }
  },
  computed: {
    allProducts() {
      return this.$store.getters.allProducts
    },
    loadingStatus() {
      return this.$store.getters.isLoading
    },
    isCartOpen() {
      return this.$store.getters.isCartOpen
    }
  },
  mounted() {
    this.$store.dispatch('fetchProducts')
    // Check mobile view when component is mounted
    // this.checkMobileView()
    window.addEventListener('resize', this.checkMobileView)
  },
  beforeUnmount() {
    window.removeEventListener('resize', this.checkMobileView)
  },
  methods: {
    checkMobileView() {
      if (window.innerWidth <= 700) {
        return (this.isMobile = true)
      }
    }
  }
}
</script>

<style></style>
