<template>
     <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <router-link class="navbar-brand" to="/">首頁</router-link>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav mr-auto">
        <li class="nav-item">
          <router-link class="nav-link" to="/">首頁</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/products">產品列表</router-link>
        </li>
        <li class="nav-item">
          <router-link class="nav-link" to="/cart">購物車</router-link>
        </li>
      </ul>
    </div>
    <button type="button" class="btn btn-primary">
      結帳
      <span class="badge rounded-pill bg-danger">{{cartData.cart.length}}</span>
    </button>
  </nav>
</template>

<script>

export default {
  data () {
    return {
      cartData: {
        cart: []
      }
    }
  },
  methods: {
    getCart () {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`).then((res) => {
        console.log('cart:', res)
        this.cartData = res.data.data
      })
    },
    mounted () {
      this.getCart()
    }
  }
}
</script>
