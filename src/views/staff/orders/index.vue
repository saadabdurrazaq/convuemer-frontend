<template>
  <div class="wrapper">
    <Nav />
    <Sidebar />
    <div class="content-wrapper">
      <!-- Title and breadacrumbs -->
      <Breadcrumbs />
      <!-- End title and breadacrumbs -->
      <div class="content">
        <div style="display: none" id="errMsg" class="box no-border">
          <div class="box-tools">
            <p class="alert alert-success alert-dismissible">
              {{ this.title }}
              <button
                type="button"
                @click.prevent="closeMsg"
                class="close"
                data-hide="alert"
                aria-label="Close"
              >
                <span aria-hidden="true">&times;</span>
              </button>
            </p>
          </div>
        </div>
        <!-- begin the element -->
        <div class="row">
          <div class="col-12">
            <div class="card card-outline card-info">
              <div class="card-body">
                <div class="row" style="margin-top: -20px">
                  <div class="col-md-6">
                    <nav class="navecation">
                      <ul id="navi" style="margin-top: 40px">
                        <li>
                          <router-link
                            :to="{ name: 'orders-index' }"
                            :class="isInAllData ? 'menu current' : 'menu'"
                            href="#"
                            >All ({{ totalRecords }})</router-link
                          >
                        </li>
                      </ul>
                    </nav>
                  </div>
                </div>
                <hr />

                <div style="float: left; padding-top: 3px; padding-right: 4px">
                  Show
                </div>
                <div style="width: 73px; float: left; margin-bottom: 7px">
                  <select
                    id="pagination"
                    class="form-control select2bs4 select2-accessible"
                    style="width: 73px"
                    data-select2-id=" 17"
                    tabindex="-1"
                    aria-hidden="true"
                    name="showEntries"
                    v-on:change="showEntries()"
                  >
                    <option value="5" data-select2-id="19">5</option>
                    <option value="10" data-select2-id="38">10</option>
                    <option value="25" data-select2-id="39">25</option>
                    <option value="50" data-select2-id="40">50</option>
                    <option value="100" data-select2-id="41">100</option>
                    <option value="250" data-select2-id="42">250</option>
                  </select>
                </div>
                <div
                  style="
                    float: left;
                    padding-top: 3px;
                    padding-left: 4px;
                    padding-right: 10px;
                  "
                >
                  entries
                </div>
                <div style="float: left; margin-left: 4px">
                  <select
                    class="
                      form-control
                      select2bs4
                      select2-accessible
                      trash_multiple
                    "
                    style="width: 130px"
                    data-select2-id=" 17"
                    tabindex="-1"
                    aria-hidden="true"
                    v-on:change="deleteMultiple()"
                  >
                    <option selected="selected" data-select2-id="19">
                      Bulk Actions
                    </option>
                    <option data-select2-id="38" value="trashMultiple">
                      Delete
                    </option>
                  </select>
                </div>
                <div style="float: right">
                  <form action="">
                    <div
                      class="input-group input-group-sm"
                      style="width: 215px"
                    >
                      <input
                        type="text"
                        @keyup="searchData"
                        placeholder="Search by invoice number"
                        v-model="search"
                        class="form-control float-right"
                      />
                      <div class="input-group-append">
                        <button
                          type="submit"
                          value="Filter"
                          class="btn btn-default"
                        >
                          <i class="fas fa-search"></i>
                        </button>
                      </div>
                    </div>
                  </form>
                </div>

                <div
                  class="panel-body table-responsive"
                  style="overflow: hidden"
                >
                  <!-- Data-table with pagination for user list. -->
                  <table
                    class="table table-bordered table-hover dataTable"
                    id="cat-table"
                  >
                    <thead>
                      <tr>
                        <th style="text-align: center">
                          <input
                            type="checkbox"
                            id="select-multiple"
                            @click="selectMultiple()"
                          />
                        </th>
                        <th>No</th>
                        <th>Invoice Number</th>
                        <th>Status</th>
                        <th>Buyer</th>
                        <th>Total Quantity</th>
                        <th>Order Date</th>
                        <th>Actions</th>
                      </tr>
                    </thead>
                    <tbody>
                      <div
                        v-if="loading"
                        style="position: absolute; top: 50%; left: 50%"
                      >
                        <div class="lds-facebook">
                          <div></div>
                          <div></div>
                          <div></div>
                        </div>
                      </div>
                      <!-- Loop through each user record and display user details -->
                      <tr
                        :class="'data-' + order.id"
                        v-for="order in orders"
                        :key="order.id"
                      >
                        <td style="text-align: center">
                          <input
                            type="checkbox"
                            id="select"
                            class="sub_chk"
                            :data-id="order.id"
                            :value="order.id"
                            name="selected_values[]"
                          />
                        </td>
                        <td class="align-middle">
                          {{
                            orders.indexOf(order) +
                            1 +
                            (currentPage - 1) * perPage
                          }}
                        </td>
                        <td>{{ order.invoice_number }}</td>
                        <td>{{ order.status }}</td>
                        <td>
                          {{ order.user.name }}
                          <small>{{ order.user.email }}</small>
                        </td>
                        <td>
                          {{ order.total_quantity }}
                        </td>

                        <td>
                          {{
                            new Intl.DateTimeFormat("en-GB", {
                              year: "numeric",
                              month: "long",
                              day: "2-digit",
                            }).format(new Date(order.created_at))
                          }}
                        </td>
                        <td style="text-align: center; width: 30%">
                          <a
                            class="btn btn-info"
                            style="margin-right: 7px"
                            href=""
                            @click.prevent="editOrder(order)"
                          >
                            <i class="fa fa-edit"></i> Edit
                          </a>
                          <a
                            class="btn btn-danger"
                            style="margin-right: 7px"
                            href=""
                            @click.prevent="forceDelete(order.id)"
                          >
                            <i class="fa fa-trash"></i> Delete
                          </a>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                  <div v-if="isOrdersPagination">
                    <nav style="margin-top: 5px" class="float-left">
                      <p>
                        Showing {{ from }} to {{ to }} of
                        {{ totalRecords }} entries
                      </p>
                    </nav>
                    <nav
                      aria-label="Page navigation example"
                      class="pagination-container float-right"
                      style="margin-top: 5px"
                    >
                      <pagination
                        v-model="page"
                        :records="totalRecords"
                        :per-page="perPage"
                        @paginate="getData"
                      />
                    </nav>
                  </div>
                  <div v-if="isShowEntriesPagination">
                    <nav style="margin-top: 5px" class="float-left">
                      <p>
                        Showing {{ from }} to {{ to }} of
                        {{ totalRecords }} entries
                      </p>
                    </nav>
                    <nav
                      aria-label="Page navigation example"
                      class="pagination-container float-right"
                      style="margin-top: 5px"
                    >
                      <pagination
                        v-model="page"
                        :records="totalRecords"
                        :per-page="perPage"
                        @paginate="showEntries"
                      />
                    </nav>
                  </div>
                  <div v-if="isSearchPagination">
                    <nav style="margin-top: 5px" class="float-left">
                      <p>
                        Showing {{ from }} to {{ to }} of
                        {{ totalRecords }} entries
                      </p>
                    </nav>
                    <nav
                      aria-label="Page navigation example"
                      class="pagination-container float-right"
                      style="margin-top: 5px"
                    >
                      <pagination
                        v-model="page"
                        :records="totalRecords"
                        :per-page="perPage"
                        @paginate="searchData"
                      />
                    </nav>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Modal containing dynamic form for adding/updating user details. -->
          <div
            class="modal fade"
            id="exampleModal"
            tabindex="-1"
            role="dialog"
            aria-labelledby="exampleModalLabel"
            aria-hidden="true"
          >
            <div class="modal-dialog modal-dialog-centered" role="document">
              <div class="modal-content">
                <div class="modal-header">
                  <!-- Show/hide headings dynamically based on /isFormCreateSliderMode value (true/false) -->
                  <h5
                    v-show="!isFormCreateSliderMode"
                    class="modal-title"
                    id="exampleModalLabel"
                  >
                    Update slider
                  </h5>
                  <button
                    type="button"
                    class="close"
                    data-dismiss="modal"
                    aria-label="Close"
                  >
                    <span aria-hidden="true">×</span>
                  </button>
                </div>

                <!-- Form for adding/updating user details. When submitted call /createSlider() function if /isFormCreateSliderMode value is true. Otherwise call /updateSlider() function. -->
                <form
                  @submit.prevent="
                    isFormCreateSliderMode ? createSlider() : updateSlider()
                  "
                >
                  <div class="modal-body">
                    <div
                      class="alert alert-danger alert-dismissible fade show"
                      role="alert"
                      style="display: none"
                    >
                      <button
                        type="button"
                        @click.prevent="closeMsg"
                        class="close"
                        data-dismiss="alert"
                        aria-label="Close"
                      >
                        <span aria-hidden="true">&times;</span>
                      </button>
                    </div>
                    <div
                      v-if="loadingForm"
                      style="position: absolute; top: 20%; left: 40%"
                    >
                      <div class="lds-facebook">
                        <div></div>
                        <div></div>
                        <div></div>
                      </div>
                    </div>
                    <div class="form-group">
                      <label for="invoice_number">Invoice Number</label>
                      <input
                        disabled
                        v-model="form.invoice_number"
                        type="text"
                        name="invoice_number"
                        id="invoice_number"
                        placeholder="Invoice Number"
                        class="form-control"
                        :class="{
                          'is-invalid': form.errors.has('invoice_number'),
                        }"
                      />
                      <span class="text-danger" id="codeError"></span>
                      <div
                        style="color: red"
                        v-if="form.errors.has('invoice_number')"
                        v-html="form.errors.get('invoice_number')"
                      />
                    </div>
                    <div class="form-group">
                      <label for="user">Buyer</label>
                      <input
                        v-model="form.user.name"
                        disabled
                        type="text"
                        name="user"
                        id="user"
                        placeholder="Buyer"
                        class="form-control"
                        :class="{
                          'is-invalid': form.errors.has('user'),
                        }"
                      />
                      <span class="text-danger" id="codeError"></span>
                      <div
                        style="color: red"
                        v-if="form.errors.has('user')"
                        v-html="form.errors.get('user')"
                      />
                    </div>
                    <div class="form-group">
                      <label for="created_at">Order Date</label>
                      <input
                        v-model="form.created_at"
                        disabled
                        :class="{
                          'is-invalid': form.errors.has('created_at'),
                        }"
                        id="created_at"
                        class="form-control"
                        name="created_at"
                        required
                        autocomplete="created_at"
                      />
                      <span class="text-danger" id="created_at_error"></span>
                      <div
                        style="color: red"
                        v-if="form.errors.has('created_at')"
                        v-html="form.errors.get('created_at')"
                      />
                    </div>
                    <div class="form-group">
                      Products ({{ form.total_quantity }}):
                      <template v-if="form.products.length > 0">
                        <ul style="list-style-type: none">
                          <li
                            v-for="product in form.products"
                            :key="product.id"
                          >
                            <a
                              :href="
                                $router.resolve({
                                  name: 'product-show',
                                  params: {
                                    id: product.url_id,
                                    slug: product.url,
                                  },
                                }).href
                              "
                            >
                              {{ product.product_name }}</a
                            >
                            ({{ product.pivot.quantity }})
                          </li>
                        </ul>
                      </template>
                      <template v-if="form.variants_prod.length > 0">
                        <ul
                          style="list-style-type: none"
                          :style="
                            form.products.length > 0 &&
                            form.variants_prod.length > 0
                              ? 'margin-top:-13px;'
                              : ''
                          "
                        >
                          <li
                            v-for="var_prod in form.variants_prod"
                            :key="var_prod.id"
                          >
                            <a
                              :href="
                                $router.resolve({
                                  name: 'product-show',
                                  params: {
                                    id: var_prod.url_id,
                                    slug: var_prod.url,
                                  },
                                }).href
                              "
                              >{{ var_prod.product_name }}</a
                            >({{ var_prod.pivot.quantity }})
                          </li>
                        </ul>
                      </template>
                    </div>
                    <div class="form-group">
                      <label for="status">Status</label>
                      <select
                        v-model="form.status"
                        type="text"
                        name="status"
                        id="status"
                        placeholder="Status"
                        class="form-control"
                        :class="{
                          'is-invalid': form.errors.has('status'),
                        }"
                      >
                        <option :value="null" disabled selected>
                          Select Role
                        </option>
                        <option
                          v-for="listValue in status"
                          :value="listValue"
                          v-bind:key="listValue"
                          :selected="listValue === form.status"
                        >
                          {{ listValue }}
                        </option>
                      </select>
                      <span class="text-danger" id="codeError"></span>
                      <div
                        style="color: red"
                        v-if="form.errors.has('status')"
                        v-html="form.errors.get('status')"
                      />
                    </div>
                  </div>
                  <div class="modal-footer">
                    <button
                      type="button"
                      class="btn btn-secondary"
                      data-dismiss="modal"
                    >
                      Close
                    </button>
                    <button
                      type="submit"
                      class="btn btn-primary"
                      v-show="isFormCreateSliderMode"
                    >
                      Save changes
                    </button>
                    <button
                      type="submit"
                      class="btn btn-primary"
                      v-show="!isFormCreateSliderMode"
                    >
                      Update
                    </button>
                  </div>
                </form>
              </div>
            </div>
          </div>
          <!-- end modal -->
        </div>
        <!-- end the element -->
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
import "admin-lte/dist/css/adminlte.min.css"; // conflict with frontend theme
import "@/assets/css/custom.css";
import jQuery from "jquery";
const $ = jQuery;
window.$ = $;
import _ from "lodash";
import swal from "sweetalert2";
import Nav from "../partials/Nav.vue";
import Breadcrumbs from "../partials/Breadcrumbs.vue";
import Sidebar from "../partials/Sidebar.vue";
import Footer from "../partials/Footer.vue";
import { Form } from "vform";
import { BASE_URL } from "@/assets/js/base-url.js";

export default {
  name: "orders-index",

  beforeCreate: function () {
    document.body.className = "home-staff";
  },

  components: {
    Nav,
    Breadcrumbs,
    Sidebar,
    Footer,
  },

  // Declare orders (as object), form (as /vform instance) and /isFormCreateSliderMode (as boolean defaulted to 'true') inside /data() { return {} }.
  data() {
    return {
      BASE_URL: BASE_URL,
      page: 1,
      perPage: 0,
      totalRecords: 0,
      from: 0,
      to: 0,
      currentPage: 0,
      search: "",
      trashed: 0,
      orders: {},
      form: new Form({
        id: "",
        invoice_number: "",
        total_quantity: "",
        created_at: "",
        user: {},
        status: "",
        products: [],
        variants_prod: [],
      }),
      msg: "",
      title: "",
      imagePreview: null,
      showPreview: false,
      isFormCreateSliderMode: true,
      isInAllData: true,
      loading: false,
      loadingForm: false,
      defaultOrdersPagination: false,
      defaultShowEntriesPagination: false,
      defaultSearchPagination: false,
      isOrdersPagination: false,
      isShowEntriesPagination: false,
      isSearchPagination: false,
      detectNewRecord: false,
      detectUpdate: false,
      detectTrash: false,
      detectMultipleTrash: false,
      selectedValues: [],
      status: ["SUBMIT", "PROCESS", "FINISH", "CANCEL"],
    };
  },

  methods: {
    updateStatus() {
      var self = this;

      $(".status").bootstrapSwitch();
      $(".status").on("switchChange.bootstrapSwitch", function (e) {
        var id = e.target.value;

        if ($(this).is(":checked") === false) {
          self.form.status = "Inactive";
        } else {
          self.form.status = "Active";
        }

        let formData = new FormData();
        formData.append("status", self.form.status);
        formData.append("_method", "PUT");

        self.form
          .put("api/staff/orders/update-status/" + id, {
            data: formData,
          })
          .then((response) => {
            console.log(response);
          })
          .catch((err) => {
            console.log(err);
          })
          .finally(() => {
            console.log("Work");
          });
      });
    },

    showSuccessMsg(response) {
      this.msg = response.message;
      $("#errMsg").fadeIn().delay(2000).fadeOut();

      const Toast = swal.mixin({
        toast: true,
        position: "top-end",
        showConfirmButton: false,
        timer: 3000,
        timerProgressBar: true,
        didOpen: (toast) => {
          toast.addEventListener("mouseenter", swal.stopTimer);
          toast.addEventListener("mouseleave", swal.resumeTimer);
        },
      });

      Toast.fire({
        icon: "success",
        title: this.title,
      });
    },

    loadData(response) {
      var responseData = response.data;
      this.orders = responseData.orders.data;
      this.totalRecords = responseData.orders.total;
      this.from = responseData.orders.from;
      this.to = responseData.orders.to;
      this.currentPage = responseData.orders.current_page;
      this.trashed = responseData.total_trashed_orders;
      this.perPage = responseData.items;
    },

    loadSpecificPage() {
      if (this.defaultOrdersPagination === true) {
        this.isOrdersPagination = true;
        this.getData(this.page);
        console.log(`I am in getData`);
      } else if (this.defaultShowEntriesPagination === true) {
        this.isShowEntriesPagination = true;
        this.showEntries(this.page);
        console.log(`I am in show entries`);
      } else if (this.defaultSearchPagination === true) {
        this.searchData(); // the page is not defined because we should return it to page 1.
        console.log(`I am in search page`);
      } else {
        this.page = 1;
        this.getData(this.page);
        console.log(`No matching option. So, I am in getData`);
      }
    },

    determineDefaultPage() {
      // for example if user trash data in show entries page, after submit he should be directed to show entries page as well with same page (if he was in page 3, he should be directed in page 3. etc...)
      if (_.isEmpty(this.search) === false) {
        this.defaultSearchPagination = true;
        this.isSearchPagination = true;
        this.defaultOrdersPagination = false;
        this.defaultShowEntriesPagination = false;
      }
    },

    highlightNewRecords() {
      // Highlight new record if new record exist after submit
      var newRecord = document.getElementById("cat-table");
      var rows = newRecord.rows;

      if (this.detectNewRecord === true) {
        var newRow = rows[0 + 1];
        $(newRow)
          .toggleClass("highlight")
          .fadeOut(2000, function () {
            $(this).toggleClass("highlight").fadeIn(5);
          });
      }

      this.detectNewRecord = false;
    },

    highlightChangedRecord() {
      if (this.detectUpdate === true || this.detectTrash === true) {
        $(".data-" + this.form.id)
          .toggleClass("highlight")
          .fadeOut(1500, function () {
            $(this).toggleClass("highlight").fadeIn(1);
          });
      } else if (this.detectMultipleTrash === true) {
        let values = this.selectedValues;
        values.forEach(function (value) {
          $(".data-" + value)
            .toggleClass("highlight")
            .fadeOut(1500, function () {
              $(this).toggleClass("highlight").fadeIn(1);
            });
        });
      }

      this.detectUpdate = false;
      this.detectTrash = false;
      this.detectMultipleTrash = false;
    },

    closeMsg() {
      $("#errMsg").hide("slow");
      document.getElementById("slider_image").value = "";
      this.imagePreview = null;
      this.showPreview = false;
      this.form.slider_image = null;
    },

    // /getData() function. Function we use to get user list by calling api/orders method GET.
    getData(page) {
      this.loading = true;
      this.isOrdersPagination = true;
      this.defaultOrdersPagination = true;
      this.defaultShowEntriesPagination = false;
      this.defaultSearchPagination = false;
      this.isShowEntriesPagination = false;
      this.isSearchPagination = false;

      const token = localStorage.getItem("token-staff");

      if (typeof page === "undefined") {
        page = 1;
      }

      this.axios.defaults.headers.common.Authorization = `Bearer ${token}`;
      this.axios
        .get("api/staff/orders/index", {
          params: {
            page: page,
          },
        })
        .then((response) => {
          this.loadData(response);
        })
        .catch((error) => {
          console.log(error);
        })
        .finally(() => {
          this.loading = false;
          this.highlightNewRecords();
          this.updateStatus();
        });
    },

    showEntries(page) {
      this.loading = true;
      this.isShowEntriesPagination = true;
      this.defaultShowEntriesPagination = true;
      this.defaultOrdersPagination = false;
      this.defaultSearchPagination = false;
      this.isOrdersPagination = false;
      this.isSearchPagination = false;

      var val = $("select[name=showEntries] option").filter(":selected").val();

      if (typeof page === "undefined") {
        this.page = 1;
      }

      this.axios
        .get("api/staff/orders/index?items=" + val, {
          params: {
            page: page,
          },
        })
        .then((response) => {
          this.search = "";
          this.loadData(response);
        })
        .catch((error) => {
          console.log(error.response.data);
          swal.fire({
            icon: "error",
            title: "Oops...",
            text: "Something went wrong!",
            footer: "<a href>Why do I have this issue?</a>",
          });
        })
        .finally(() => {
          this.loading = false;
          this.highlightNewRecords();
        });
    },

    searchData: _.debounce(function (page) {
      if (_.isEmpty(this.search)) {
        this.defaultSearchPagination = false;
        this.isSearchPagination = false;
        this.loadSpecificPage();
      } else {
        this.loading = true;

        if (this.defaultOrdersPagination === true) {
          this.isOrdersPagination = false;
        } else if (this.defaultShowEntriesPagination === true) {
          this.isShowEntriesPagination = false;
        }

        this.isSearchPagination = true;

        if (typeof page === "undefined") {
          page = 1;
        }

        this.axios
          .get("api/staff/orders/search/" + this.search, {
            params: {
              page: page,
            },
          })
          .then((response) => {
            this.loadData(response);
          })
          .catch((error) => {
            console.log(error);
          })
          .finally(() => {
            this.loading = false;
            this.updateStatus();
          });
      }
    }),

    // /showModal() function. Function we use to 1. Set /isFormCreateSliderMode to 'true', 2. Reset form data, 3. Show modal containing dynamic form for adding/updating user details.
    showModal() {
      this.isFormCreateSliderMode = true;
      this.form.reset(); // v form reset
      $("#exampleModal").modal("show"); // show modal
    },

    // /editUser() function. Function we use to 1. Set /isFormCreateSliderMode to 'false', 2. Reset and clear form data, 3. Show modal containing dynamic form for adding/updating user details, 4. Fill form with user details.
    editOrder(order) {
      this.isFormCreateSliderMode = false;
      this.form.reset(); // v form reset inputs
      this.form.clear(); // v form clear errors
      this.loadingForm = false;
      $("#exampleModal").modal("show"); // show modal
      this.form.fill(order);
      this.form.created_at = new Intl.DateTimeFormat("en-GB", {
        year: "numeric",
        month: "long",
        day: "2-digit",
      }).format(new Date(this.form.created_at));
      console.log(this.form);
    },

    // /updateSlider() function. Function we use to update user details by calling api/orders/{id} method PUT (carrying form input data).
    updateSlider() {
      let myXhr;
      const token = localStorage.getItem("token-staff");
      var self = this;

      // if input image is empty, set null and false. So that, data can be updated without attaching an image.
      if (document.getElementById("slider_image").value == "") {
        self.imagePreview = null;
        self.showPreview = false;
        self.form.slider_image = null;
      }

      let formDataUpdate = new FormData();
      formDataUpdate.append("slider_header", this.form.slider_header);
      formDataUpdate.append("title", this.form.title);
      formDataUpdate.append("description", this.form.description);
      formDataUpdate.append("status", this.form.status);
      formDataUpdate.append("link", this.form.link);
      formDataUpdate.append("slider_image", this.form.slider_image);
      formDataUpdate.append("_method", "PUT");
      let selectedImage = $("#slider_image")[0].files[0];
      formDataUpdate.append("selectedImages", selectedImage);

      $.ajaxSetup({
        url: `${BASE_URL}/api/staff/orders/update/` + this.form.id,
      });

      $.ajax({
        method: "POST",
        data: formDataUpdate,
        enctype: "multipart/form-data",
        cache: false,
        contentType: false,
        processData: false,
        dataType: "JSON",
        async: true,
        headers: {
          "Content-Type": undefined,
        },
        xhr: function () {
          myXhr = $.ajaxSettings.xhr();
          return myXhr;
        },
        beforeSend: function (xhr) {
          xhr.setRequestHeader("Authorization", `Bearer ${token}`);
          self.loadingForm = true;
        },
        success: function (result) {
          if (result.errors) {
            console.log(result.errors);
            // sweet alert fail
            swal.fire({
              icon: "error",
              title: "Oops...",
              text: "Something went wrong!",
              footer: "<a href>Why do I have this issue?</a>",
            });
          } else {
            $("#exampleModal").modal("hide"); // hide modal

            self.detectUpdate = true;
            self.highlightChangedRecord();
            self.determineDefaultPage();
            self.loadSpecificPage();
            self.title = "Slider updated successfully!";
            self.showSuccessMsg(result);
            self.loadingForm = false;
            self.imagePreview = null;
            self.showPreview = false;
            document.getElementById("slider_image").value = "";
            self.form.slider_image = null;
          }
        },
        error: function (response) {
          self.loadingForm = false;
          console.log(response.responseJSON.errors.title);
          $("#title").addClass("is-invalid");
          $("#codeError").text(response.responseJSON.errors.title);
          $(".alert-danger").html("An error in your input fields");
          $.each(response.responseJSON.errors, function (key, value) {
            $(".alert-danger").show();
            $(".alert-danger").append(
              "<strong><li>" + value + "</li></strong>"
            );
          });
        },
      });
    },

    forceDelete(id) {
      this.form.id = id;

      swal
        .fire({
          title: "Are you sure?",
          text: "All relationship data will be deleted. You won't be able to revert this!",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Yes, delete it!",
        })
        .then((result) => {
          if (result.value) {
            this.form
              .delete("api/staff/orders/force-delete/" + id, {})
              .then((response) => {
                this.detectDelete = true;
                this.highlightChangedRecord();
                this.determineDefaultPage();
                this.loadSpecificPage();
                this.title = "Order deleted successfully!";
                this.showSuccessMsg(response);
              })
              .catch(() => {
                // sweet alert fail
                swal.fire({
                  icon: "error",
                  title: "Oops...",
                  text: "Something went wrong!",
                  footer: "<a href>Why do I have this issue?</a>",
                });
              });
          }
        });
    },

    selectMultiple() {
      var checked = $("#select-multiple").is(":checked", true);
      if (checked) {
        $(".sub_chk").prop("checked", true);
      } else {
        $(".sub_chk").prop("checked", false);
      }
    },

    deleteMultiple() {
      if ($(".trash_multiple").val() == "trashMultiple") {
        var allVals = [];
        $(".sub_chk:checked").each(function () {
          allVals.push($(this).attr("data-id"));
        });

        this.selectedValues = allVals;

        if (allVals.length <= 0) {
          alert("Please select row.");
        } else {
          swal
            .fire({
              title: "Are you sure?",
              text: "All will be deleted permanently!",
              icon: "warning",
              showCancelButton: true,
              confirmButtonColor: "#3085d6",
              cancelButtonColor: "#d33",
              confirmButtonText: "Yes, trash it!",
            })
            .then((result) => {
              if (result.isConfirmed) {
                var join_selected_values = allVals.join(",");
                this.axios
                  .get(
                    "api/staff/orders/force-delete-multiple/ids=" +
                      join_selected_values
                  )
                  .then((response) => {
                    this.detectMultipleTrash = true;
                    this.highlightChangedRecord();
                    this.determineDefaultPage();
                    this.loadSpecificPage();
                    this.title = "Orders has been deleted successfully!";
                    this.showSuccessMsg(response);
                  })
                  .catch((error) => {
                    console.log(error.response.data);
                    swal.fire({
                      icon: "error",
                      title: "Oops...",
                      text: "Something went wrong!",
                      footer: "<a href>Why do I have this issue?</a>",
                    });
                  });
              } else {
                swal.fire("Cancelled", "Your data is safe :)", "error");
              }
            });
        }
      }
    },
  },

  created() {
    // Call /getData() function initially.
    this.getData();
  },

  mounted() {
    console.log("Component mounted.");
    // prevent sweetalert error if user change the route when swal is still visible.
    if (swal.isVisible()) {
      document
        .querySelector("body")
        .setAttribute("class", "swal2-toast-shown swal2-shown");
    }
  },
};
</script>

<style type="scss">
.highlight {
  background: #fff2e1;
}
.pagination-container {
  height: 75px;
  display: grid;
}
.pagination {
  margin: auto !important;
}
.bootstrap-switch-small,
.bootstrap-switch,
.bootstrap-switch-wrapper,
.bootstrap-switch-focused,
.bootstrap-switch-animate,
.bootstrap-switch-off {
  width: 60.4063px;
}

.bootstrap-switch-container {
  width: 85.2032px;
  margin-left: -26.7969px;
}

.bootstrap-switch-handle-on,
.bootstrap-switch-primary {
  width: 42.8px;
}

.bootstrap-switch-label {
  width: 46.6094px;
}

.bootstrap-switch-handle-off,
.bootstrap-switch-danger {
  width: 42.8px;
}
</style>
