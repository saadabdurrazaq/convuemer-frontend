<template>
  <Nav />
  <div class="body-content outer-top-xs" id="top-banner-and-menu">
    <div class="container">
      <div class="row">
        <FrontSidebar />
        <!-- ============================================== CONTENT ============================================== -->
        <div class="col-xs-12 col-sm-12 col-md-9 homebanner-holder">
          <!-- ========================================== SECTION – HERO ========================================= -->
          <ImgCarousel />
          <!-- ========================================= SECTION – HERO : END ========================================= -->

          <!-- ============================================== INFO BOXES ============================================== -->
          <div class="info-boxes wow fadeInUp">
            <div class="info-boxes-inner">
              <div class="row">
                <div class="col-md-6 col-sm-4 col-lg-4">
                  <div class="info-box">
                    <div class="row">
                      <div class="col-xs-12">
                        <h4 class="info-box-heading green">money back</h4>
                      </div>
                    </div>
                    <h6 class="text">30 Days Money Back Guarantee</h6>
                  </div>
                </div>
                <!-- .col -->

                <div class="hidden-md col-sm-4 col-lg-4">
                  <div class="info-box">
                    <div class="row">
                      <div class="col-xs-12">
                        <h4 class="info-box-heading green">free shipping</h4>
                      </div>
                    </div>
                    <h6 class="text">Shipping on orders over $99</h6>
                  </div>
                </div>
                <!-- .col -->

                <div class="col-md-6 col-sm-4 col-lg-4">
                  <div class="info-box">
                    <div class="row">
                      <div class="col-xs-12">
                        <h4 class="info-box-heading green">Special Sale</h4>
                      </div>
                    </div>
                    <h6 class="text">Extra $5 off on all items</h6>
                  </div>
                </div>
                <!-- .col -->
              </div>
              <!-- /.row -->
            </div>
            <!-- /.info-boxes-inner -->
          </div>
          <!-- ============================================== INFO BOXES : END ============================================== -->

          <!-- ============================================== Featured Products ============================================== -->
          <FeaturedProducts />
          <!-- ============================================== End Featured Products ============================================== -->

          <!-- ============================================== WIDE PRODUCTS ============================================== -->
          <div class="wide-banners wow fadeInUp outer-bottom-xs">
            <div class="row">
              <div class="col-md-12">
                <div class="wide-banner cnt-strip">
                  <div class="image">
                    <img
                      class="img-responsive"
                      :src="`${BASE_URL}/storage/app/public/banners/home-banner.jpg`"
                      alt=""
                    />
                  </div>
                  <div class="strip strip-text">
                    <div class="strip-inner">
                      <h2 class="text-right">
                        New Mens Fashion<br />
                        <span class="shopping-needs">Save up to 40% off</span>
                      </h2>
                    </div>
                  </div>
                  <div class="new-label">
                    <div class="text">NEW</div>
                  </div>
                  <!-- /.new-label -->
                </div>
                <!-- /.wide-banner -->
              </div>
              <!-- /.col -->
            </div>
            <!-- /.row -->
          </div>
          <!-- ============================================== WIDE PRODUCTS : END ============================================== -->

          <ProductList />
        </div>

        <!-- ============================================== CONTENT : END ============================================== -->
      </div>
      <!-- /.row -->
    </div>
    <!-- /.container -->
  </div>
  <Footer />
</template>

<script>
import "@/assets/frontend/css/bootstrap.min.css";
import "@/assets/frontend/css/main-blue-green.css";
import "@/assets/frontend/css/animate.min.css";
import "@/assets/frontend/css/blue-green.css";
import "@/assets/frontend/css/bootstrap-select.min.css";
import "@/assets/frontend/css/bootstrap.min.css";
import "@/assets/frontend/css/font-awesome.css";
import "@/assets/frontend/css/lightbox.css";
import "@/assets/frontend/css/loading.css";
import "@/assets/frontend/css/owl.carousel.css";
import "@/assets/frontend/css/owl.transitions.css";
import "@/assets/frontend/css/rateit.css";
import jQuery from "jquery";
const $ = jQuery;
window.$ = $;
import Nav from "./partials/Nav.vue";
import Footer from "./partials/Footer.vue";
import FrontSidebar from "./partials/FrontSidebar.vue";
import { BASE_URL } from "@/assets/js/base-url.js";
import ImgCarousel from "./partials/ImgCarousel.vue";
import FeaturedProducts from "./partials/FeaturedProducts.vue";
import ProductList from "./partials/ProductList.vue";

export default {
  beforeCreate: function () {
    document.body.className = "welcome";
    document.body.classList.remove("login", "hold-transition", "sidebar-mini");
    document.body.classList.add("cnt-home");
  },

  components: {
    Nav,
    Footer,
    FrontSidebar,
    ImgCarousel,
    FeaturedProducts,
    ProductList,
  },

  data() {
    return {
      BASE_URL: BASE_URL,
    };
  },

  methods: {},
  created() {},
  mounted() {},
};
</script>

<style>
/* .agile__nav-button {
  border: none;
  color: #888;
  cursor: pointer;
  font-size: 14px;
  top: -45px;
  transition-duration: 0.3s;
}
.thumbnails .agile__nav-button {
  position: absolute;
  transform: translateY(-50%);
}
.thumbnails .agile__nav-button--prev {
  right: 35px;
}
.thumbnails .agile__nav-button--next {
  right: 5px;
}
.thumbnails .fa-chevron-right {
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 7px;
  padding-right: 7px;
  background: #dddddd;
}
.thumbnails .fa-chevron-left {
  padding-top: 5px;
  padding-bottom: 5px;
  padding-left: 7px;
  padding-right: 7px;
  background: #dddddd;
}
.agile__nav-button:hover {
  color: #888;
} */
</style>
