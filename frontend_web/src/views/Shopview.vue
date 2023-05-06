<script>
import SliderShop from "@/components/SliderShop.vue";
import HeaderShop from "../components/HeaderShop.vue";
import FooterShop from "../components/FooterShop.vue";
import ProductService from "../services/Product.service";
import { mapState } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
import toastsVue from "../components/toasts.vue";
export default {
  data() {
    return {
      Products: [],
    };
  },
  components: {
    SliderShop,
    FooterShop,
    HeaderShop,
    toastsVue,
  },
  computed: {
    ...mapState(useAuthStore, {
      currentUser: "user",
    }),
  },
  methods: {
    async retrieveProduct() {
      try {
        this.Products = await ProductService.getAll();
      } catch (error) {
        console.log(error);
      }
    },
  },
  mounted() {
    this.retrieveProduct();
  },
};
</script>
<template>
  <div>
    <toastsVue></toastsVue>
    <HeaderShop></HeaderShop>
    <div class="slider">
      <SliderShop></SliderShop>
    </div>
    <div style="margin: 20px 100px">
      <div style="text-align: center; margin: 30px 0" class="heading">
        <h2>Áo phông</h2>
      </div>
      <div class="flex-row" style="margin: 0 100px">
        <div class="d-sm-flex flex-wrap" id="AoPhong">
          <div
            class="card m-1"
            style="width: 16rem"
            v-for="item in Products"
            :key="item.id"
            v-show="item.categories === 'Phông'"
          >
            <router-link
              :to="{
                name: 'details',
                params: { id: item._id },
              }"
              class="wrapper-img"
            >
              <div class="image_slider">
                <div class="image_item" v-for="img in item.img" :key="img">
                  <img :src="img" class="card-img-top" alt="..." />
                </div>
              </div>
            </router-link>
            <div class="card-body product">
              <router-link
                style="color: black; font-size: 18px; text-decoration: none"
                :to="{
                  name: 'details',
                  params: { id: item._id },
                }"
                class="card-title"
                >{{ item.title }}</router-link
              >
              <h6 class="price">{{ item.price }}$</h6>
            </div>
          </div>
        </div>
        <div style="text-align: center; margin: 30px 0" class="heading">
          <h2>Áo Polo</h2>
        </div>
        <div class="d-sm-flex flex-wrap" id="Polo">
          <div
            class="card m-1"
            style="width: 16rem"
            v-for="item in Products"
            :key="item.id"
            v-show="item.categories === 'Polo'"
          >
            <router-link
              :to="{
                name: 'details',
                params: { id: item._id },
              }"
              class="wrapper-img"
            >
              <div class="image_slider">
                <div class="image_item" v-for="img in item.img" :key="img">
                  <img :src="img" class="card-img-top" alt="..." />
                </div>
              </div>
            </router-link>
            <div class="card-body product">
              <router-link
                style="color: black; font-size: 18px; text-decoration: none"
                :to="{
                  name: 'details',
                  params: { id: item._id },
                }"
                class="card-title"
                >{{ item.title }}</router-link
              >
              <h6 class="price">{{ item.price }}$</h6>
            </div>
          </div>
        </div>
        <div style="text-align: center; margin: 30px 0" class="heading">
          <h2>Áo Hoodie</h2>
        </div>
        <div class="d-sm-flex flex-wrap" id="Hoodie">
          <div
            class="card m-1"
            style="width: 16rem"
            v-for="item in Products"
            :key="item.id"
            v-show="item.categories === 'Hoodie'"
          >
            <router-link
              :to="{
                name: 'details',
                params: { id: item._id },
              }"
              class="wrapper-img"
            >
              <div class="image_slider">
                <div class="image_item" v-for="img in item.img" :key="img">
                  <img :src="img" class="card-img-top" alt="..." />
                </div>
              </div>
            </router-link>
            <div class="card-body product">
              <router-link
                style="color: black; font-size: 18px; text-decoration: none"
                :to="{
                  name: 'details',
                  params: { id: item._id },
                }"
                class="card-title"
                >{{ item.title }}</router-link
              >
              <h6 class="price">{{ item.price }}$</h6>
            </div>
          </div>
        </div>
      </div>
    </div>
    <FooterShop></FooterShop>
  </div>
</template>
<style scoped>
.wrapper-img {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.image_slider {
  display: flex;
  transition: all 0.8s ease;
}
.image_slider:hover {
  transform: translateX(-100%);
}
.image_item {
  flex: 1 0 100%;
}
</style>
