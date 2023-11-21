<script setup>

</script>

<template>
  <div class="hero">
    <h1 class="big-title">Som hittat</h1>
  </div>
  <div class="filter-sort-wrapper">
    <div>
      <input
        @input="this.products = searchProducts(this.searchResult)"
        placeholder="Sök efter produkt..."
        type="text"
        v-model="searchResult"
      />
    </div>

    <div class="filter-sort-container">
      <div class="filter-container">
        <p>Visa:</p>
        <select v-model="category" @change="filterCategory(this.category)">
          <option>Allt</option>
          <option>Hittegods</option>
          <option>Kläder</option>
          <option>Skor</option>
          <option>Elektronik</option>
          <option>Glasögon</option>
        </select>
      </div>

      <div class="sort-container">
        <button class="sort-button" @click="priceLow">
          Lågt till högt pris
        </button>
        <button class="sort-button" @click="priceHigh">
          Högt till lågt pris
        </button>
        <button class="sort-button" @click="alfabeticalHigh">A-Ö</button>
        <button class="sort-button" @click="alfabeticalLow">Ö-A</button>
      </div>
    </div>

  </div>
  <div class="products-wrapper">
    <div class="products-container">
      <ProductItem
        class="product-item"
        v-for="product in products"
        :key="product.id"
        @click="selectProduct(product.id)"
        :image="product.image"
        :price="product.price"
        :title="product.title"
      />
    </div>
  </div>
</template>

<style scoped>

p,
h1,
select,
button{
    margin: 0;
    padding: 0;
}
.hero {
  min-height: 500px;
  background-color: var(--primary-red);
  display: flex;
  justify-content: center;
  align-items: center;
}

.big-title {
  font-family: "Turret Road", cursive;
  font-weight: bold;
  font-size: 4em;
  margin: 0 auto;
  text-align: center;
  background-color: white;
  padding: 1rem;
}

@media (min-width: 768px) {
  .big-title {
    font-size: 6em;
  }
}

@media (min-width: 1024px) {
  .big-title {
    font-size: 8em;
  }
}

.filter-sort-wrapper {
  margin: 0.4rem;
  display: flex;
  justify-content: space-between;
}

.filter-sort-wrapper input:focus {
  border: 2px solid transparent;
  outline: 2px solid var(--primary-red);
}

.filter-sort-container {
  display: flex;
}

.filter-container {
  display: flex;
}

.filter-container select{
    border: none;
}
.sort-button {
}

.product-item {
  cursor: pointer;
}

.products-wrapper {
  background-color: white;
  padding-top: 5rem;
}

.products-container {
  position: relative;
  display: grid;
  grid-template-columns: 1fr;
}

@media (min-width: 375px) {
  .products-container {
    grid-template-columns: 1fr 1fr;
  }
}

@media (min-width: 550px) {
  .products-container {
    grid-template-columns: 1fr 1fr 1fr;
  }
}

@media (min-width: 768px) {
  .products-container {
    grid-template-columns: 1fr 1fr 1fr 1fr;
  }
}

@media (min-width: 1024px) {
  .products-container {
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  }
}
</style>

<script>
import ProductItem from "../components/ProductItem.vue";
import axios from "axios";
export default {
  components: {
    ProductItem,
  },
  mounted() {
    this.fetchProducts();
  },
  // emits: ["produkt-vald"],
  methods: {
    priceHigh() {
      this.products.sort((a, b) => b.price - a.price);
      console.log("körs denna funktion? priceHigh");
    },
    alfabeticalHigh() {
      this.products.sort((a, b) => a.title.localeCompare(b.title));
      console.log("körs denna funktion? AlfabeticalHigh");
    },
    alfabeticalLow() {
      this.products.sort((a, b) => a.title.localeCompare(b.title)).reverse();
      console.log("körs denna funktion? AlfabeticalLow");
    },
    priceLow() {
      this.products.sort((a, b) => a.price - b.price);
      console.log("körs denna funktion? priceLow");
    },

    // Filter funktionalitet
    async filterCategory(kategory) {
      console.log(kategory);
      if (kategory === "Allt") {
        await this.fetchProducts();
      } else if (kategory !== "Allt") {
        await this.fetchProducts();
        this.products = this.products.filter(
          (item) => item.category === kategory
        );
      }
    },

    // Sök funktionalitet
    searchProducts(search) {
      console.log(search);
      // const matchingProducts = this.products.filter((product) => {
      //   const title = product.title.toLowerCase();
      //   return title.includes(search.toLowerCase());
      // });
      // if (matchingProducts.length == 0) {
      //   console.log(matchingProducts == true);
      //   console.log(matchingProducts.length);
      //   console.log(this.resterProducts.length);
      //   console.log("Nu ska man fetcha nya produkter");

      const matchandeProdukter = this.resterProducts.filter((produkt) => {
        const titel = produkt.title.toLowerCase();
        return titel.includes(search.toLowerCase());
      });
      console.log(matchandeProdukter.length, " vi kom till andra sök");
      return matchandeProdukter;
      // } else {
      //   console.log("vi kom till else, nu ska man fetcha enligt sök");
      //   console.log(matchingProducts, typeof matchingProducts);
      //   return matchingProducts;
      // }
    },
    mounted() {
      fetch("/productapi.json")
        .then((Response) => Response.json())
        .then((productapi) => {
          this.products = productapi;
        });
    },

    selectProduct(id) {
      // const valdProdukt = this.products.find(product => product.id == id)
      // console.log(valdProdukt.title)
      // console.log(id); name: 'productdetail' Voalr föreslåt detta "name: ProductDetailView" props: { productData: this.products }
      // this.$emit("produkt-vald", { id, productData: valdProdukt });
      this.$router.push({
        name: "productdetail",
        params: { productID: id },
      });
    },
    async fetchProducts() {
      const result = await axios.get("productapi.json", {
        headers: {
          Accept: "application/json",
        },
      });
      this.products = result.data;
      this.resterProducts = result.data;
    },
  },
  data() {
    return {
      products: [],
      category: "Allt",
      searchResult: "",
      resterProducts: [],

      // filteredArray: []
      // Kategori: "Glasögon" || "Skor" || "Kläder" || "Hittegods" || "Elektronik",
    };
  },
};
</script>
