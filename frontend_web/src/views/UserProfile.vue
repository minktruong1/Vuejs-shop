<script>
import { mapState } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
import HeaderShop from "../components/HeaderShop.vue";
import FooterShop from "../components/FooterShop.vue";
import EditUser from "../components/EditUser.vue";
import toastsVue from "../components/toasts.vue";
export default {
  data() {
    return {
      checkedit: false,
    };
  },
  components: {
    EditUser,
    HeaderShop,
    FooterShop,
    toastsVue,
  },
  computed: {
    ...mapState(useAuthStore, {
      currentUser: "user",
    }),
  },
  methods: {
    showedit() {
      if (!this.checkedit) {
        this.checkedit = true;
      } else {
        this.checkedit = false;
      }
    },
  },
};
</script>
<template>
  <div>
    <toastsVue></toastsVue>
    <HeaderShop></HeaderShop>
    <h1 class="text-center">Thông tin cá nhân</h1>
    <div style="display: flex" class="container,">
      <div v-if="currentUser" style="margin: 100px">
        <div class="row">
          <div class="col-md-8">
            <p class="text-break">
              <strong>Name:</strong>
              {{ currentUser.username }}
            </p>
            <p>
              <strong>E-mail:</strong>
              {{ currentUser.email }}
            </p>
          </div>
        </div>
        <button
          class="btn btn-warning text-light"
          style="margin-left: 20px"
          @click="showedit"
        >
          Chỉnh sửa
        </button>
      </div>
      <div
        v-if="checkedit"
        style="display: flex; flex-direction: column; justify-content: center"
      >
        <EditUser :user="currentUser"></EditUser>
      </div>
    </div>
    <FooterShop></FooterShop>
  </div>
</template>
