<script>
import toastjs from "../assets/js/toasts";
import toastsVue from "../components/toasts.vue";
import ProductService from "../services/Product.service";
import HeaderShop from "../components/HeaderShop.vue";
import FooterShop from "../components/FooterShop.vue";
import Productform from "../components/Productform.vue";
export default {
  data() {
    return {
      toasts: {
        title: "Success",
        msg: "Sửa sản phẩm thành công",
        type: "success",
        duration: 2000,
      },
      product: null,
    };
  },
  components: {
    HeaderShop,
    FooterShop,
    Productform,
    toastsVue,
  },
  methods: {
    toastjs,
    async getproduct(id) {
      try {
        this.product = await ProductService.get(id);
      } catch (error) {
        console.log(error);
        this.$router.push({
          name: "notfound",
          params: { pathMatch: this.$route.path.split("/").slice(1) },
          query: this.$route.query,
          hash: this.$route.hash,
        });
      }
    },
    async updateProduct(data) {
      try {
        await ProductService.update(this.product._id, data);
        this.toastjs();
      } catch (error) {
        console.log(error);
        (this.toasts.title = "Warning"),
          (this.toasts.msg = "Tài khoản không phải ADMIN"),
          (this.toasts.type = "warn"),
          (this.toasts.duration = 2000);
        this.toastjs();
      }
    },
  },
  created() {
    this.getproduct(this.$route.params.id);
  },
};
</script>
<template>
  <div>
    <HeaderShop></HeaderShop>
    <toastsVue></toastsVue>
    <div v-if="product" class="page">
      <h3 class="text-center">Chỉnh sửa sản phẩm</h3>
      <Productform :product="product" @submit:product="updateProduct" />
    </div>
    <FooterShop></FooterShop>
  </div>
</template>
