<script>
import HeaderShop from "../components/HeaderShop.vue";
import FooterShop from "../components/FooterShop.vue";
import * as yup from "yup";
import { Form, Field, ErrorMessage } from "vee-validate";
import AuthService from "../services/Auth.service";
import toast from "../assets/js/toasts";
import toastsVue from "../components/toasts.vue";
export default {
  components: {
    HeaderShop,
    FooterShop,
    Form,
    Field,
    ErrorMessage,
    toastsVue,
  },
  data() {
    const signupform = yup.object().shape({
      name: yup
        .string()
        .required("Tên phải có giá trị.")
        .min(2, "Tên phải ít nhất 2 ký tự."),
      email: yup
        .string()
        .required("Email phải có giá trị.")
        .email("E-mail không đúng.")
        .max(50, "E-mail tối đa 50 ký tự."),
      pwd: yup.string().required("Mật khẩu phải có giá trị."),
    });
    return {
      signupform,
      message: "Đăng ký thành công",
      usernew: {
        username: "",
        email: "",
        password: "",
      },
      toasts: {
        title: "",
        msg: "",
        type: "",
        duration: 0,
      },
    };
  },
  methods: {
    toast,
    async postuser() {
      try {
        await AuthService.createsignup(this.usernew);
        (this.toasts.title = "Success"),
          (this.toasts.msg = "Đăng ký thành công"),
          (this.toasts.type = "success"),
          (this.toasts.duration = 2000);
        this.toast();
        setTimeout(() => {
          this.$router.push({ name: "login" });
        }, 2000);
      } catch (erorr) {
        console.log(erorr);
        (this.toasts.title = "Faild"),
          (this.toasts.msg = "Thông tin bạn nhập đã được dùng"),
          (this.toasts.type = "error"),
          (this.toasts.duration = 2000);
        this.toast();
      }
    },
  },
};
</script>

<template>
  <div>
    <toastsVue></toastsVue>
    <HeaderShop></HeaderShop>
    <section class="text-center text-lg-start">
      <div style="width: 50%" class="container py-4">
        <div class="row g-0 align-items-center">
          <div
            class="cascading-right"
            style="
              background: hsla(0, 0%, 100%, 0.55);
              backdrop-filter: blur(30px);
            "
          >
            <div class="p-5 shadow-5">
              <h2 class="fw-bold mb-5 text-center">Signup</h2>
              <Form :validation-schema="signupform">
                <div class="form-outline mb-4">
                  <label class="form-label" for="name">Account Name</label>
                  <Field
                    id="name"
                    name="name"
                    type="text"
                    class="form-control"
                    v-model="usernew.username"
                  />
                  <ErrorMessage name="name" class="text-danger" />
                </div>
                <div class="mb-4">
                  <label class="form-label" for="email">Email</label>
                  <div class="form-outline">
                    <Field
                      id="email"
                      name="email"
                      type="email"
                      class="form-control"
                      v-model="usernew.email"
                    />
                    <ErrorMessage name="email" class="text-danger" />
                  </div>
                </div>
                <div class="form-outline mb-4">
                  <label class="form-label" for="pwd">Password</label>
                  <Field
                    id="pwd"
                    name="pwd"
                    type="password"
                    class="form-control"
                    v-model="usernew.password"
                  />
                  <ErrorMessage name="pwd" class="text-danger" />
                </div>

                <!-- Submit button -->
                <button
                  type="button"
                  class="btn btn-primary btn-block mb-4"
                  @click="postuser()"
                >
                  Signup
                </button>
                <p>
                  Already signup?<router-link to="/login">
                    Login now</router-link
                  >
                </p>
              </Form>
            </div>
          </div>
        </div>
      </div>
      <!-- Jumbotron -->
    </section>
    <FooterShop></FooterShop>
  </div>
  <!-- Section: Design Block -->
</template>
<style scoped>
.cascading-right {
  margin-right: -50px;
}
@media (max-width: 991.98px) {
  .cascading-right {
    margin-right: 0;
  }
}
</style>
