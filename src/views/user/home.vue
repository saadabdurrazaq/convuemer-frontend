<template v-if="isLoggedIn">
  <Nav />
  <div class="body-content outer-top-xs" id="top-banner-and-menu">
    <div class="container">
      <div class="row">
        <ProfileSidebar />
        <div class="col-xs-12 col-sm-12 col-md-9 homebanner-holder">
          <div class="row">
            <div class="col-md-12">
              <div
                class="card"
                style="
                  background-color: white;
                  margin-top: 20px;
                  margin-bottom: 20px;
                "
              >
                <br />
                <ul class="nav nav-tabs" style="margin-left: 10px">
                  <li :class="$route.path.includes('home') ? 'active' : ''">
                    <a
                      data-toggle="tab"
                      class="home"
                      @click="createDeepLinking('home')"
                      href="#home"
                      >Home</a
                    >
                  </li>
                  <li><a data-toggle="tab" href="#menu1">Profile</a></li>
                  <li
                    :class="
                      $route.path.includes('shipping-addresses') ? 'active' : ''
                    "
                  >
                    <a
                      data-toggle="tab"
                      class="link_address"
                      @click="createDeepLinking('link_address')"
                      href="#menu2"
                      >List Addresses</a
                    >
                  </li>
                  <li><a data-toggle="tab" href="#menu3">Menu 3</a></li>
                </ul>

                <div class="tab-content">
                  <div
                    id="home"
                    :class="
                      $route.path.includes('home')
                        ? 'tab-pane fade in active'
                        : 'tab-pane fade'
                    "
                  >
                    <h3>Home</h3>
                    <p>
                      Lorem ipsum dolor sit amet, consectetur adipisicing elit,
                      sed do eiusmod tempor incididunt ut labore et dolore magna
                      aliqua.
                    </p>
                  </div>
                  <div id="menu1" class="tab-pane fade">
                    <h3>Profile</h3>
                    <p>
                      Ut enim ad minim veniam, quis nostrud exercitation ullamco
                      laboris nisi ut aliquip ex ea commodo consequat.
                    </p>
                  </div>
                  <div
                    id="menu2"
                    :class="
                      $route.path.includes('shipping-addresses')
                        ? 'tab-pane fade in active'
                        : 'tab-pane fade'
                    "
                  >
                    <h3></h3>
                    <ShippingAddresses />
                  </div>
                  <div id="menu3" class="tab-pane fade">
                    <h3>Menu 3</h3>
                    <p>
                      Eaque ipsa quae ab illo inventore veritatis et quasi
                      architecto beatae vitae dicta sunt explicabo.
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- /.row -->
    </div>
    <!-- /.container -->
  </div>
  <Footer />
</template>

<script>
import "@/assets/frontend/css/bootstrap.min.css";
import "@/assets/frontend/css/main-blue-green.css"; /* header */
import "@/assets/frontend/css/blue-green.css"; /* .header-style-1 .header-nav */
import "@/assets/frontend/css/animate.min.css";
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
import ProfileSidebar from "./partials/ProfileSidebar.vue";
import Footer from "./partials/Footer.vue";
import { useRouter } from "vue-router";
import axios from "axios";
import ShippingAddresses from "./partials/ShippingAddresses.vue";
import { mapGetters } from "vuex";

export default {
  beforeCreate: function () {
    document.body.className = "home-user";
  },
  components: {
    Nav,
    ProfileSidebar,
    Footer,
    ShippingAddresses,
  },
  computed: {
    ...mapGetters({
      user: "auth/user",
    }),
  },
  data() {
    return {
      userData: "",
    };
  },
  methods: {
    createDeepLinking(link) {
      let newUrl;
      let url = location.href.replace(/\/$/, "");
      const hash = $("." + link).attr("href");
      if (hash == "#home") {
        newUrl = url.split("#")[0];
      } else {
        newUrl = url.split("#")[0] + hash;
      }
      newUrl += "";
      history.replaceState(null, null, newUrl);
    },
    checkAuth() {
      const router = useRouter();

      // state token
      const token = this.user.token;

      if (!token) {
        return router.push({
          name: "user-login",
        });
      }

      //get data user
      axios.defaults.headers.common.Authorization = `Bearer ${token}`;
      axios
        .get("api/user")
        .then((response) => {
          this.userData = response.data;
        })
        .catch((error) => {
          console.log(error.response.data);
          if (error.response.status === 403) {
            router.push("/user/login");
          }
          return Promise.reject(error);
        });
    },
  },
  beforeMount() {},
  created() {},
  mounted() {
    this.checkAuth();
  },
};
</script>

<style>
</style>
