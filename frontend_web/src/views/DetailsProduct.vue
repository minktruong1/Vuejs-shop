<script>
import HeaderShop from "@/components/HeaderShop.vue";
import FooterShop from "../components/FooterShop.vue";
import ProductService from "../services/Product.service";
import CartService from "../services/Cart.service";
import toastsVue from "../components/toasts.vue";
import toastsjs from "../assets/js/toasts.js";
import { mapState } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
export default {
  data() {
    return {
      detailproduct: [],
      cartitem: {
        userId: "",
        productId: this.$route.params.id,
        quantity: 1,
        title: "",
        img: "",
        price: "",
        size: "",
        color: "",
      },
      carts: [],
      toasts: {
        title: "Success",
        msg: "Thêm vào giỏ hàng thành công",
        type: "success",
        duration: 2000,
      },
      sub_quantity: 1,
    };
  },
  computed: {
    ...mapState(useAuthStore, {
      currentUser: "user",
    }),
  },
  components: {
    HeaderShop,
    FooterShop,
    toastsVue,
  },
  methods: {
    toastsjs,
    async getproduct() {
      try {
        this.detailproduct = await ProductService.get(this.$route.params.id);
        this.cartitem.title = this.detailproduct.title;
        this.cartitem.img = this.detailproduct.img[0];
        this.cartitem.price = this.detailproduct.price;
        this.cartitem.size = this.detailproduct.size;
        this.cartitem.color = this.detailproduct.color;
      } catch (error) {
        console.log(error);
      }
    },
    async getidcart() {
      this.cartitem.quantity = this.sub_quantity;
      var exitcart = false;

      try {
        this.carts = await CartService.get(this.currentUser._id);
        this.cartitem.userId = this.currentUser._id;
        this.carts.map((cartproduct) => {
          if (cartproduct.productId == this.cartitem.productId) {
            this.cartitem.quantity += cartproduct.quantity;
            CartService.update(cartproduct._id, this.cartitem);
            exitcart = true;
            this.toastsjs();
            setTimeout(() => {
              this.$router.push({ name: "CartShop" });
            }, 1000);
          }
        });
        if (exitcart === false) {
          this.cartitem.userId = this.currentUser._id;
          CartService.create(this.cartitem);
          this.toastsjs();
          setTimeout(() => {
            this.$router.push({ name: "CartShop" });
          }, 1000);
        }
      } catch (error) {
        (this.toasts.title = "Message"),
          (this.toasts.msg = "Bạn chưa đăng nhập!"),
          (this.toasts.type = "warn"),
          (this.toasts.duration = 3000),
          this.toastsjs();
        console.log(error);
      }
    },
  },
  created() {
    this.getproduct();
  },
};
</script>
<template>
  <div>
    <HeaderShop></HeaderShop>
    <toastsVue></toastsVue>

    <div style="margin-top: 14vh; margin-bottom: 14vh" class="container">
      <div class="d-flex">
        <div class="img_product col">
          <div
            id="carouselExampleControls"
            class="carousel slide"
            data-bs-ride="carousel"
          >
            <div
              class="carousel-inner"
              style="
                box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
                border-radius: 8px;
              "
            >
              <div
                class="carousel-item"
                v-for="(img, index) in detailproduct.img"
                :class="{ active: index == 0 }"
                :key="index"
              >
                <img :src="img" class="d-block w-100" alt="..." style="" />
              </div>
            </div>
            <button
              class="carousel-control-prev"
              type="button"
              data-bs-target="#carouselExampleControls"
              data-bs-slide="prev"
            >
              <span
                class="carousel-control-prev-icon"
                aria-hidden="true"
              ></span>
              <span class="visually-hidden">Previous</span>
            </button>
            <button
              class="carousel-control-next"
              type="button"
              data-bs-target="#carouselExampleControls"
              data-bs-slide="next"
            >
              <span
                class="carousel-control-next-icon"
                aria-hidden="true"
              ></span>
              <span class="visually-hidden">Next</span>
            </button>
          </div>
        </div>
        <div class="info_product col" style="margin-left: 20px">
          <h2 style="font-weight: 600">{{ detailproduct.title }}</h2>
          <h4 class="pricte text-danger">{{ detailproduct.price }}$</h4>
          <div class="size_product">
            <h5>Size</h5>
            <div class="list_btn_size">
              <h6 class="text-secondary">{{ detailproduct.size }}</h6>
            </div>
          </div>
          <div class="color_product">
            <h5>Màu sắc</h5>
            <div class="list_btn_color">
              <h6 class="text-secondary">{{ detailproduct.color }}</h6>
            </div>
          </div>
          <div class="quatitly-product">
            <h5>Nhập số lượng</h5>
            <div class="col-md-3 col-lg-3 col-xl-2 d-flex">
              <input
                id="quantity"
                name="quantity"
                type="number"
                v-model="sub_quantity"
                class="form-control form-control-sm"
                style="width: 50px; border: 1px solid black"
              />
            </div>
          </div>

          <div class="describe_product col">
            <h4>Mô tả sản phẩm</h4>
            <p>{{ detailproduct.desc }}</p>
          </div>
          <div class="btn_product">
            <button
              type="submit"
              class="btn btn-danger"
              style="width: 150px"
              @click="getidcart()"
            >
              Thêm giỏ hàng
            </button>
          </div>
        </div>
      </div>
    </div>
    <FooterShop></FooterShop>
  </div>
</template>
<style scoped>
.btn_product {
  display: flex;
  justify-content: space-around;
}
.size_product,
.color_product {
  display: flex;
  flex-direction: column;
  max-width: 400px;
}
.list_btn_size,
.list_btn_color {
  margin: 0 10px;
  display: flex;
  flex-wrap: wrap;
}
.list_btn_size button,
.list_btn_color button {
  margin: 10px 5px;
}
.btn {
  width: 100px;
}

.heading {
  margin: 0 100px;
}
.title {
  display: flex;
  justify-content: center;
  flex-direction: column;
  width: 100%;
  height: 100px;
}
</style>
