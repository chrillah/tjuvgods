<template>
  <div class="container">
    <div class="top-title-container">
      <h2 class="top-title">Varukorgen</h2>
      <button @click="$store.commit('toggleCart')" class="close">
        &times;
      </button>
    </div>
    <div class="cart-container">
      <CartitemsContainer />
    </div>
    <div v-if="$store.state.cart.length === 0" class="to-checkout-container">
      <p>Du har inga stöldgods i varukorgen</p>
      <RouterLink
        @click="$store.commit('toggleCart')"
        class="action-btn link"
        to="/products"
        >Handla istället</RouterLink
      >
    </div>
    <div v-if="$store.state.cart.length" class="to-checkout-container">
      <div class="total-cost-container">
        <p>Totalt :</p>

        <p>{{ getCartTotal }} KR</p>
      </div>
      <div @click="tillKassan" class="action-btn" role="button">
        <RouterLink
          @click="$store.commit('toggleCart')"
          class="link"
          to="/summary"
        >
          Till kassan</RouterLink
        >
      </div>
    </div>
  </div>
  <!-- <div class="close-cart-area" @click="$store.commit('closeCart')">

  </div> -->
</template>

<script>
import CartitemsContainer from "./CartitemsContainer.vue";
import { mapGetters } from "vuex";
// import { V_SHOW } from "@vue/compiler-dom";
export default {
  computed: {
    ...mapGetters(["getCartTotal"]),
  },
  components: {
    CartitemsContainer,
  },
  methods: {
    tillKassan() {
      this.$router.push("/summary");
    },
  },
};
</script>

<style scoped>
.close-cart-area {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: 0;
}
.link {
  text-decoration: none;
  color: var(--black);
}

.close {
  margin-right: 1rem;
  background: none;
  border: none;
  font-size: 40px;
  padding-bottom: 1rem;
}
.container {
  top: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  min-height: 560px;
  width: 100%;
  max-width: 300px;
  position: fixed;
  box-shadow: var(--shadow);
  background-color: white;
  z-index: 999;
}

.top-title-container {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #000000;
}

.top-title {
  font-weight: 900;
  font-family: "Turret Road", cursive;
  font-size: 1.2rem;
}

.cart-container {
  width: 100%;
  height: 280px;
  overflow-y: scroll;
}
.to-checkout-container {
  width: 100%;
  display: grid;
  margin-bottom: 2rem;
}
.total-cost-container {
  display: flex;
  justify-content: space-between;
  border-bottom: 1px solid #000000;
  margin: 1rem 0;
}
</style>
