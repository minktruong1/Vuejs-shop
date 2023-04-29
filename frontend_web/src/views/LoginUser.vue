<script>
import HeaderShop from "../components/HeaderShop.vue";
import FooterShop from "../components/FooterShop.vue";
import FormloginVue from "../components/Formlogin.vue";
import toastsVue from "../components/toasts.vue";
export default {
  components: {
    HeaderShop,
    FooterShop,
    FormloginVue,
    toastsVue,
  },
};
</script>
<template>
  <div>
    <toastsVue></toastsVue>
    <HeaderShop></HeaderShop>
    <h1 class="text-center" style="margin-top: 100px; margin-left: 100px">
      Login
    </h1>
    <div class="h-100">
      <div class="container h-custom">
        <div class="row d-flex justify-content-center align-items-center h-100">
          <div style="width: 90vh">
            <FormloginVue></FormloginVue>
          </div>
        </div>
      </div>
    </div>
    <FooterShop></FooterShop>
  </div>
</template>
