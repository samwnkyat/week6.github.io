<template>
  <div class="container">
  <h2>購物車</h2>
  <table class="table align-middle">
    <thead>
      <tr>
        <th>圖片</th>
        <th>商品名稱</th>
        <th>價格</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="product in products" :key="product.id">
        <td style="width: 200px">
          <div
            :style="{ backgroundImage: `url(${product.imageUrl})` }"
            style="
              height: 100px;
              background-size: cover;
              background-position: center;
            "
          ></div>
        </td>
        <td>
          {{ product.title }}
        </td>
        <td>
         <div class="h5" v-if="product.price === product.origin_price">
            {{ product.price }} 元
          </div>
          <div v-else>
            <del class="h6">原價 {{ product.origin_price }} 元</del>
            <div class="h5">現在只要 {{ product.price }} 元</div>
          </div>
        </td>
        <td>
          <div class="btn-group btn-group-sm">
            <button
              type="button"
              class="btn btn-outline-secondary"
              @click="openProductModal(product.id)"
              :disabled="isLoadingItem === product.id"
            >
              查看更多
            </button>
            <button
              type="button"
              class="btn btn-danger"
              @click="addToCart(product.id)"
              :disabled="isLoadingItem === product.id"
            >
              <span
                class="spinner-grow spinner-grow-sm"
                v-show="isLoadingItem === product.id"
              ></span>
              加到購物車
            </button>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
  <div class="text-end">
                    <button class="btn btn-outline-danger" type="button" @click="deleteAllCarts">清空購物車</button>
                </div>

                <table class="table align-middle">
                    <thead>
                        <tr>
                            <th></th>
                            <th>品名</th>
                            <th style="width: 150px">數量/單位</th>
                            <th>單價</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!--購物車資料是否存在-->
                        <template v-if="cartData.carts">
                            <tr v-for="item in cartData.carts" :key="item.id">
                                <td>
                                    <button type="button" class="btn btn-outline-danger btn-sm"
                                        @click="removeCartItem(item.id)">
                                        x
                                    </button>
                                </td>
                                <td>
                                    {{ item.product.title }}
                                    <!-- <div class="text-success">
                                        已套用優惠券
                                    </div> -->
                                </td>
                                <td>
                                    <div class="input-group input-group-sm">
                                        <div class="input-group mb-3">
                                            <!-- <input min="1" type="number" class="form-control" v-model="item.qty"> -->
                                            <select id="" class="form-select" v-model="item.qty"
                                                @change="updateCartItem(item)">
                                                <option :value="num" v-for="num in 20" :key="`${num}-${item.id}`">
                                                    {{ num }}
                                                </option>
                                            </select>
                                            <span class="input-group-text" id="basic-addon2">{{ item.product.unit
                                                }}</span>
                                        </div>
                                    </div>
                                </td>
                                <td class="text-end">
                                    {{ item.total }}
                                </td>
                            </tr>
                        </template>
                    </tbody>
                    <tfoot>
                        <tr>
                            <td colspan="3" class="text-end">總計</td>
                            <td class="text-end">{{ cartData.total }}</td>
                        </tr>
                        <!-- <tr>
                            <td colspan="3" class="text-end text-success">折扣價</td>
                            <td class="text-end text-success">{{ }}</td>
                        </tr> -->
                    </tfoot>
                </table>
  </div>
</template>

<script>
import emitter from '@/libs/emitter'
export default {
  data () {
    return {
      products: [],
      cartData: {},
      isLoadingItem: ''
    }
  },
  methods: {
    getProducts () {
      this.$http(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/products/all`)
        .then(res => {
          this.products = res.data.products
        })
    },
    addToCart (id, qty = 1) {
      const data = {
        product_id: id,
        qty
      }
      this.isLoadingItem = id
      this.$http.post(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`, { data })
        .then((res) => {
          this.getCart()
          this.isLoadingItem = ''
          emitter.emit('get-cart')
        })
    },
    getCart () {
      this.$http.get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`).then((res) => {
        this.cartData = res.data
      })
    },
    deleteAllCarts () {
      this.$http.delete(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/carts`).then((response) => {
        alert(response.data.message)
        this.getCart()
      }).catch((err) => {
        alert(err.data.message)
      })
    }
  },
  mounted () {
    this.getProducts()
    emitter.on('get-cart', () => {
      this.getCart()
    })
  }
}
</script>
