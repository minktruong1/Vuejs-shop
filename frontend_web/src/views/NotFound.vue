<script>
import { mapState } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
import HeaderShop from "../components/HeaderShop.vue";
import FooterShop from "../components/FooterShop.vue";
export default {
  data() {
    return {
      checkedit: false,
    };
  },
  components: {
    HeaderShop,
    FooterShop,
  },
};
</script>
<template>
  <div>
    <HeaderShop></HeaderShop>
    <div className="d-flex align-items-center justify-content-center vh-100">
      <div className="text-center">
        <h1 className="display-1 fw-bold">404</h1>
        <p className="fs-3">
          <span className="text-danger">Opps!</span> Page not found.
        </p>
        <p className="lead">The page you’re looking for doesn’t exist.</p>
        <router-link to="/" class="text-body">
          <button class="btn btn-primary">Go Home</button>
        </router-link>
      </div>
    </div>
    <FooterShop></FooterShop>
  </div>
</template>
