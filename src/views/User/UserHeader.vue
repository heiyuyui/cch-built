<template>
  <div class="sticky-top">
    <nav class="navbar navbar-expand-lg bg-white border-bottom">
      <div class="container">
        <router-link class="home" to="/">
          <img src="https://diz36nn4q02zr.cloudfront.net/webapi/imagesV3/Original/SalePage/7617927/0/637810401328700000?v=1" alt height="40" />
          <h1 class="sr-only">cch</h1>
        </router-link>
        <button
          class="navbar-toggler navbar-toggle-bar"
          type="button"
          data-toggle="collapse"
          data-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <i class="fas fa-bars"></i>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav ml-auto">
            <li class="nav-item">
              <router-link class="nav-link header-navbtn text-center" to="/introduce">關於cch</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link header-navbtn text-center" to="/products">商品列表</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link header-navbtn text-center" to="/favorite">cch的最愛</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link header-navbtn text-center" to="/coupons">cch活動</router-link>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>
<script>
import $ from 'jquery'
export default {
  data () {
    return {}
  },
  created () {
    $(function () {
      if ($(window).width() < 992) {
        $('.navbar .nav-link').on('click', function () {
          $('.navbar-toggler').click()
        })
      }
    })
  }
}
</script>
