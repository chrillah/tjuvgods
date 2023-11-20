<script setup>
import { RouterLink, RouterView } from "vue-router";
import CartContainer from "../components/CartContainer.vue";
</script>

<script>
import { mapGetters } from "vuex";
import FooterBar from "./FooterBar.vue";
export default {
  data() {
    return {
      someThingInCart: true,
      menu: 'close-menu',
    };
  },
  computed: {
    ...mapGetters(["getCartItemCount"]),
    displays() {
      return this.$store.state.cart;
    },
  },
  methods:{
    openMenu(){
        this.menu = 'link-container';
    },
    closeMenu(){
        this.menu = 'close-menu';
    }
  }
};
</script>

<template>
  <nav class="nav-bar-wrapper">
    <div class="nav-bar-container">
      <RouterLink to="/">
        <img
          class="img-logo"
          src="../assets/img/tjuvgods_logo.png"
          alt="Logo"
        />
      </RouterLink>
      <div class="link-wrapper">
        <div :class="menu">
          <ul class="link-menu">
            <li class="nav-item">
              <RouterLink to="/" class="nav-link active" aria-current="page"
                >Hem</RouterLink
              >
            </li>
            <li class="nav-item">
              <RouterLink to="/products" class="nav-link">Produkter</RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink to="/about" class="nav-link">Om oss</RouterLink>
            </li>
          </ul>
          <button @click="closeMenu()" class="navbar-button" type="button">X</button>
        </div>
        <a @click="$store.commit('toggleCart')" class="cart" href="#">
          <img class="cart-logo" src="../assets/img/shopping-cart-icon-2.png" />
          <p class="cart-number" v-if="getCartItemCount > 0">
            {{ getCartItemCount }}
          </p>
        </a>
        <button @click="openMenu()" class="navbar-button" type="button">=</button>
      </div>
    </div>

    <CartContainer v-if="$store.state.showCart" />
  </nav>
  <div class="add-margin"></div>
  <RouterView />
  <FooterBar />
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Outfit:wght@300&display=swap");

button,
li,
a,
ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

a {
  text-decoration: none;
  color: black;
}
.nav-bar-wrapper {
  z-index: 999;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  background-color: #ffffff;
}

.nav-bar-container {
  max-width: 2400px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  padding: 0.5rem;
}

.link-wrapper {
  display: flex;
  justify-content: space-between;
  gap: 1rem;
}
.link-menu {
  gap: 10px;
}

nav ul li a:hover {
  color: black;
}

.img-logo {
  height: auto;
  width: 100px;
}

.cart-logo {
  height: 28px;
  width: auto;
}

.cart {
  display: grid;
  grid-template-rows:10px;
}

.cart-number {
  font-size: 10px;
  height: 16px;
  width: 16px;
  /* margin-top: 5px;
  margin-left: 3px; */
  margin-left: 6px;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #000000;
  background-color: var(--third-red);
}

.navbar-button {
  display: grid;
  font-size: 24px;
  font-weight: bolder;
}

@media (max-width: 767px) {
    .close-menu{
        height: 0;
        transition: all 150ms ease-in-out;
        overflow: hidden;
    }
  .link-container {
    height: auto;
    position: fixed;
    background-color: #ffffff;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    transition: all 150ms ease-in-out;
    padding: 0.5rem;
  }
}

@media (min-width: 768px) {
  .navbar-button {
    display: none;
  }

  .link-menu {
    display: flex;
    justify-content: space-around;
    gap: 10px;
  }
}

.add-margin {
  margin-top: 3.5rem;
}
</style>
