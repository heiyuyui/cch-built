<template>
  <div>
    <loading :active.sync="isLoading"></loading>
    <div class="container my-5">
      <div class="row">
        <div class="col-md-3">
          <div class="input-group mb-3">
            <input
              type="text"
              class="form-control btn-round"
              @keyup.enter="searchProducts"
              v-model.trim="keyPoint"
              placeholder="搜尋商品"
            />
            <div class="input-group-append">
              <button class="btn btn-primary btn-round" @click="searchProducts">
                <i class="fas fa-search"></i>
              </button>
            </div>
          </div>
          <ul class="ul-category">
            <li>
              <a
                class="h5"
                :class="{'active':category==='All'}"
                @click.prevent="changeCategory('All')"
              >全部商品</a>
            </li>
            <li v-for="(item, index) in filterCategory" :key="index">
              <a
                class="h5"
                :class="{'active':category===item}"
                @click.prevent="changeCategory(item)"
              >{{ item }}</a>
            </li>
          </ul>
        </div>
        <div class="col-md-9 mt-4 mt-md-0">
          <div v-if="products.length">
            <div class="text-center h2" v-if="filterProducts.length===0">目前無此商品</div>
          </div>
          <div class="row">
            <div
              class="col-md-6 col-lg-4 mb-4"
              data-aos="fade-up"
              v-for="(item) in filterProducts"
              :key="item.id"
            >
              <div class="card shadow-sm card-round">
                <router-link :to="{path : `/product/${item.id}` }" class="pic">
                  <div :style="{backgroundImage :`url(${item.image})`}" class="pic-enlarge"></div>
                </router-link>
                <router-link :to="{path : `/product/${item.id}` }" class="card-body text-decoration-none">
                  <h5 class="card-title text-dark">{{ item.title }}</h5>
                  <div class="d-flex justify-content-between align-items-baseline">
                    <div
                      class="h4 text-dark"
                      v-if="item.price===item.origin_price"
                    >{{ item.origin_price | currency }}</div>
                    <del
                      class="h6 text-secondary"
                      v-if="item.price!=item.origin_price"
                    >{{ item.origin_price | currency }}</del>
                    <div
                      class="h4 text-danger"
                      v-if="item.price!=item.origin_price"
                    >{{ item.price | currency }}</div>
                  </div>
                  <div class="d-flex justify-content-between align-items-baseline">
                    <a class="text-primary" @click.prevent="addMyFavorite(item.id)" title="加入最愛">
                      <i class="far fa-heart fa-lg" :class="{'fas fa-heart fa-lg':item.isLike}"></i>
                    </a>
                    <a class="text-primary" @click.prevent="addtoCart(item.id)" title="加入購物車">
                      <i class="fas fa-cart-plus fa-2x"></i>
                    </a>
                  </div>
                </router-link>
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
  data () {
    return {
      category: 'All',
      categoryItem: [],
      keyPoint: '',
      search: false
    }
  },
  methods: {
    getProducts () {
      this.$store.dispatch('getProducts')
    },
    getCart () {
      this.$store.dispatch('getCart')
    },
    addtoCart (id, qty = 1) {
      this.$store.dispatch('addToCart', { id: id, qty: qty })
    },
    addMyFavorite (id) {
      this.$store.dispatch('addMyFavorite', id)
    },
    changeCategory (item) {
      const vm = this
      vm.keyPoint = ''
      vm.category = item
    },
    searchProducts () {
      const vm = this
      vm.search = true
    }
  },
  computed: {
    filterProducts () {
      const vm = this
      if (vm.search) {
        vm.search = false
        return vm.products.filter(function (item) {
          return item.title.match(vm.keyPoint)
        })
      } else if (vm.category === 'All') {
        return vm.products
      } else if (vm.category !== 'All') {
        return vm.products.filter(function (item) {
          return item.category === vm.category
        })
      } else {
        return vm.products
      }
    },
    filterCategory () {
      const vm = this
      vm.products.forEach(item => {
        vm.categoryItem.push(item.category)
      })
      return vm.categoryItem.filter(function (item, index, arr) {
        return arr.indexOf(item) === index
      })
    },
    isLoading () {
      return this.$store.state.isLoading
    },
    products () {
      return this.$store.state.products
    },
    myFavorite () {
      return this.$store.state.myFavorite
    }
  },
  created () {
    const vm = this
    vm.getProducts()
    vm.getCart()
    vm.$bus.$on('goAllProducts', category => {
      vm.category = category
    })
  }
}
</script>
