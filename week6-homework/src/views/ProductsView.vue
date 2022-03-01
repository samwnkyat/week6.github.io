<template>
  <h2>產品列表</h2>
  <div class="containter">
    <!-- row決定內層數量 -->
  <div class="row row-col-1 row-cols-lg-4 g-3"><!-- row決定內層數量 -->
  <div class="col"  v-for="product in products" :key="product.id">
    <div class="card h-100">
  <img :src="product.imageUrl" class="card-img-top" alt="...">
  <div class="card-body">
    <h5 class="card-title">{{ product.title }}</h5>
    <p class="card-text">
      {{ product.description }}
      </p>
    <router-link :to="`/product/${product.id}`" class="btn btn-primary">
      Go somewhere
    </router-link>
  </div>
</div>
  </div>
  </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      products: []
    }
  },
  methods: {
    getProducts () {
      this.$http(`${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`)
        .then(res => {
          this.products = res.data.products
        })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
