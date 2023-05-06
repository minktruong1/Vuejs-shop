<script>
import CartService from "../services/Cart.service";
import { mapState, mapActions } from "pinia";
import { useAuthStore } from "@/stores/Auth.store";
import toast from "../assets/js/toasts";
export default {
  data() {
    return {
      carts: [],
      toasts: {
        title: "Warning",
        msg: "Bạn chưa đăng nhập",
        type: "warn",
        duration: 3000,
      },
    };
  },
  computed: {
    ...mapState(useAuthStore, {
      currentUser: "user",
    }),
    getlengthcarts() {
      return this.carts.length;
    },
  },
  methods: {
    toast,
    ...mapActions(useAuthStore, ["logout", "loadAuthState"]),

    async showcarts() {
      try {
        this.showuser();
        if (this.currentUser != null) {
          this.carts = await CartService.get(this.currentUser._id);
        }
      } catch (error) {
        console.log(error);
      }
    },
    showuser() {
      if (this.currentUser == null) {
        document.querySelector(".login").style.display = "none";
        document.querySelector(".not-login").style.display = "block";
      } else {
        document.querySelector(".login").style.display = "block";
        document.querySelector(".not-login").style.display = "none";
        document.querySelector(".data_user").innerHTML =
          this.currentUser.username;
      }
    },
    handlelogout() {
      this.logout();
      this.$router.push({ name: "login" });
    },
    gotocart() {
      if (!this.currentUser) {
        this.toast();
      } else {
        this.$router.push({ name: "CartShop" });
      }
    },
  },
  created() {
    this.loadAuthState();
  },
  mounted() {
    this.showcarts();
  },
};
</script>
<template>
  <div>
    <header
      style="background-color: #f1f0f1"
      class="navbar navbar-expand-lg navbar-dark"
    >
      <div class="container">
        <a class="navbar-brand" href="/">
          <img
            src="https://drive.google.com/uc?id=1ZxkjBxFFiS-Tdi6VKxRSFG76BevGUeao"
            alt="Vue shop icon"
            style="width: 220px"
          />
        </a>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <li class="nav-item">
              <router-link to="/" class="nav-link" aria-current="page">
                Trang Chủ
              </router-link>
            </li>
            <li class="nav-item dropdown">
              <a
                class="nav-link dropdown-toggle"
                href="#"
                id="navbarDropdown"
                role="button"
                data-bs-toggle="dropdown"
                aria-expanded="false"
              >
                Sản phẩm
              </a>
              <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
                <li><a class="dropdown-item" href="#AoPhong">Áo phông</a></li>
                <li><a class="dropdown-item" href="#Polo">Polo</a></li>
                <li><a class="dropdown-item" href="#Hoodie">Hoodie</a></li>
              </ul>
            </li>
            <div id="search__area">
              <form className="d-flex" role="search" onSubmit="{handleSubmit}">
                <input
                  className=" me-2"
                  type="search"
                  placeholder="Tìm kiếm"
                  aria-label="Search"
                />
                <button className="" type="submit">
                  <i
                    style="font-size: 16px; color: white"
                    class="bi bi-search icon"
                  >
                  </i>
                </button>
              </form>
            </div>
          </ul>

          <div class="User">
            <div class="not-login">
              <i
                class="bi bi-person-circle icon"
                data-bs-toggle="collapse"
                href="#user"
              ></i>
              <div class="collapse user_link" id="user">
                <div class="card card-body connect-shop">
                  <router-link style="color: black" to="/login"
                    >Đăng nhập</router-link
                  >
                  <router-link style="color: black" to="/signup"
                    >Đăng ký</router-link
                  >
                </div>
              </div>
            </div>
            <div class="login">
              <div>
                <span
                  style="cursor: pointer"
                  class="data_user"
                  data-bs-toggle="collapse"
                  href="#user"
                >
                  <i style="margin-left: 4px" class="bi bi-caret-down-fill"></i>
                </span>
              </div>
              <div class="collapse user_link" id="user">
                <div
                  style="background-color: white"
                  class="card card-body connect-shop"
                >
                  <router-link to="/profile" style="color: black"
                    >Trang cá nhân</router-link
                  >
                  <a
                    to="/"
                    style="color: black; cursor: pointer"
                    @click="handlelogout()"
                    >Đăng xuất</a
                  >
                </div>
              </div>
            </div>
          </div>

          <div class="Cart">
            <div class="wrapper_cart">
              <div class="cart_link" id="cart_link">
                <div
                  style="
                    background-color: white;
                    box-shadow: 0 -4px 32px rgba(0, 0, 0, 0.2);
                  "
                  class="card card-body cart-info"
                >
                  <h3>Your cart</h3>

                  <div class="cart-list">
                    <div style="text-decoration: none">
                      <div class="item_cart" v-for="item in carts" :key="item">
                        <div class="item-img">
                          <img :src="item.img" class="img-product" alt="" />
                        </div>
                        <div class="item-name">
                          <span style="color: black" class="name-product">{{
                            item.title
                          }}</span>
                        </div>
                        <div class="item-price">
                          <span style="color: black" class="price-product"
                            >{{ item.price }}$</span
                          >
                        </div>
                        <div class="item-quantity">
                          <span style="color: black" class="quantity-product"
                            >x{{ item.quantity }}</span
                          >
                        </div>
                      </div>
                    </div>
                  </div>
                  <div class="footer-cart">
                    <span class="text-light"
                      >Có
                      <span class="lenghtCart">{{ getlengthcarts }}</span> sản
                      phẩm trong giỏ hàng
                    </span>
                    <button
                      class="btn btn-danger"
                      type="button"
                      @click="gotocart"
                    >
                      Vào giỏ hàng
                    </button>
                  </div>
                </div>
              </div>
              <i class="bi bi-cart-fill icon icon_cart"></i>
              <span class="quantity_cart">{{ getlengthcarts }}</span>
            </div>
          </div>
        </div>
      </div>
    </header>
  </div>
</template>
<style scoped>
header {
  border-bottom: 1px solid #d4d4d4;
}
.connect-shop {
  padding: 0;
}
.connect-shop a:hover {
  background-color: rgb(243, 243, 244, 4);
}
.connect-shop a {
  padding: 10px 10px;
}
.navbar-dark .navbar-nav .nav-link {
  color: black;
}

/* Header search bar */
#search__area {
  position: relative;
  background-color: white;
  padding: 5px 15px;
  border-radius: 30px;
  margin-left: 24px;
}

#search__area input {
  border: none;
  width: 100%;
}

#search__area input:focus {
  outline: none !important;
}

#search__area button {
  color: white;
  border: none;
  background-color: #ea1c00;
  position: absolute;
  width: 40px;
  height: 100%;
  border-radius: 0 30px 30px 0;
  top: 0;
  right: 0;
}
.User,
.Cart {
  margin-left: 20px;
}
.wrapper_cart {
  position: relative;
  width: 50px;
  height: 50px;
}

.wrapper_cart::after {
  content: "";
  position: absolute;
  display: none;
  top: 25px;
  left: -4px;
  color: transparent;
  border-width: 20px;
  border-style: solid;
}

.wrapper_cart:hover::after {
  display: block;
}

.quantity_cart {
  position: absolute;
  top: 5px;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  line-height: 20px;
  color: #fff;
  font-size: 16px;
  background: #ee4266;
  right: 10px;
  text-align: center;
}
.cart_link {
  position: absolute;
  top: 110%;
  right: 0;
  width: 500px;
  display: none;
  z-index: 100;
  overflow-y: scroll;
  max-height: 500px;
}
#cart_link::-webkit-scrollbar {
  width: 6px;
  background-color: #f5f5f5;
}
.wrapper_cart:hover > .cart_link {
  cursor: pointer;
  display: block;
}

.cart-info {
  width: 100%;
  height: 100%;
  overflow-y: auto;
}
.cart-info::-webkit-scrollbar {
  width: 3px;
  background-color: #fff;
}
.cart-info::-webkit-scrollbar-thumb {
  background-color: #acacac;
  border-radius: 6px;
}
.hidden {
  visibility: hidden;
}
.input_search {
  visibility: visible;
  animation: Search 0.5s linear;
}

.user_link {
  width: 150px;
  text-align: start;
  position: absolute;
  top: 97%;
  right: 170px;
  z-index: 10;
}
.user_link a {
  display: block;
  text-decoration: none;
}
@keyframes Search {
  0% {
    transform: translateX(5%);
  }
  100% {
    transform: translateX(0%);
  }
}
.item_cart {
  display: flex;
  justify-content: space-around;
  margin: 40px 0;
}
.item-img {
  margin-right: 10px;
}
.img-product {
  width: 100px;
  height: 100px;
  border-radius: 5px;
}
.item-name {
  width: 250px;
}
.name-product {
  white-space: wrap;
  min-width: 50px;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}
.item_cart:hover {
  background-color: rgb(243, 243, 244, 4);
}
.footer-cart {
  display: flex;
  justify-content: space-around;
}
@media only screen and (max-width: 1024px) {
  .User,
  .Cart {
    display: none;
  }
  @keyframes Search {
    0% {
      transform: translateX(5%);
    }
    100% {
      transform: translateX(0%);
    }
  }
}
@keyframes fadeIn {
  0% {
    opacity: 0.5;
    transform: translateY(-100%);
  }
  100% {
    opacity: 1;
    transform: translateY(0%);
  }
}
</style>
