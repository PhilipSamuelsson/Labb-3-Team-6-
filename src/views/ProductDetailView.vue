<template>
  <button @click="backToProducts" class="back-to">
    <div class="symbol-container">
      <img src="../assets/img/arrow.png" alt="Arrow vector" />
    </div>
    <p>Produkter</p>
  </button>
  <div class="container">
    <div class="row">
      <div class="col-lg-8">
        <img :src="varan.image" alt="product image" class="produktbilden" />
      </div>
      <div class="col-lg-4">
        <h4 class="titel">{{ varan.title }}</h4>
        <p>
          Beskrivning: <br />
          {{ varan.description }}
        </p>
        <h4>Kategori: {{ varan.category }}</h4>
        <h4 class="pris">Pris {{ varan.price }} Kr</h4>
        <h4>Lagersaldo: {{ varan.quantity }}</h4>
        <button v-if="disabledAddButton" class="kopKnapp action-btn" @click="varuID">
          Lägg i Kundkorg
        </button>
        <button @click="remove" v-if="!disabledAddButton" class="kopKnapp action-btn">
          Ta bort från varukorgen
        </button>
      </div>
    </div>
  </div>
  <!--
  <div class="information"></div>
  <div class="mer-information"></div>
  <div class="mera-information">
    <h4 class="titel">{{ varan.title }}</h4>
    <p>
      Beskrivning: <br />
      {{ varan.description }}
    </p>
    <h4>Kategori: {{ varan.category }}</h4>
    <h4 class="pris">Pris {{ varan.price }} Kr</h4>
    <h4>Lagersaldo: {{ varan.quantity }}</h4>
    <button v-if="disabledAddButton" class="kopKnapp action-btn" @click="varuID">
      Lägg i Kundkorg
    </button>
    <button @click="remove" v-if="!disabledAddButton" class="kopKnapp action-btn">
      Ta bort från varukorgen
    </button>
  </div>
  -->
  <!-- <div class="information"></div>
  <div class="mer-information"></div> -->

  <!-- <div class="similar-product-container">
    <SmallProductContainer
      @idFromSmallProduct="toProductdetail"
      :similar-products="listOfSimilarProducts"
    />
  </div> -->
</template>

<script>
import axios from "axios";
// import SmallProductContainer from "../components/SmallProductContainer.vue";
import { mapMutations } from "vuex";
export default {
  //   components: { SmallProductContainer },
  data() {
    return {
      listOfSimilarProducts: [],
      productID: this.$route.params.productID,
      varan: {},
      noProductAdded: true,
    };
  },
  computed: {
    disabledAddButton() {
      for (let i = 0; i < this.$store.state.cart.length; i++) {
        if (this.varan.id === this.$store.state.cart[i].id) {
          return false;
        }
      }
      return true;
    },
  },
  methods: {
    toProductdetail(id) {
      this.refreshComponent();
      this.$router.push({
        name: "productdetail",
        params: { productID: id },
      });
    },
    backToProducts() {
      this.$router.push("/products");
    },
    async fetchProduct() {
      const result = await axios.get("/productapi.json", {
        headers: {
          Accept: "application/json",
        },
      });
      this.listOfSimilarProducts = result.data;
      this.varan = result.data[this.productID - 1];
    },
    ...mapMutations(["addItemCart", "removeFromCart"]),
    varuID() {
      this.addItemCart(this.varan);
    },
    remove() {
      this.removeFromCart(this.varan.id);
    },
    refreshComponent() {
      console.log("hej");
      this.$forceUpdate();
      console.log(this.$forceUpdate);
    },
  },
  created() {
    this.fetchProduct();
  },
};
</script>

<style>
button {
  border: none;
  background: none;
}
.back-to {
  margin-top: 1rem;
  display: grid;
  grid-template-columns: 40px 1fr;
}
.symbol-container {
  max-width: 30px;
}
.symbol-container img {
  width: 100%;
  height: auto;
}
.kontägner {
  display: flex;
  /* flex-direction: column; */
  flex-direction: row;
  align-items: center;
  margin-top: 5vh;
}

.produktbilden {
  width: clamp(250px, 80%, 920px);
  margin-bottom: 5vh;
  margin-top: 0;
  height: 80vh;
  box-sizing: content-box;
  object-fit: contain;
  margin-left: 5.5vh;
}

.information {
  width: clamp(250px, 80%, 920px);
  display: flex;
  margin: 3vh 5vh;
  justify-content: space-between;
}

.information .kopKnapp {
  background-color: white;
  padding: clamp(1.5%, 2%, 3%) clamp(1.5%, 4%, 7%);
  font-size: clamp(12px, 3vw, 20px);
  border: 3px black solid;
  font-weight: bolder;
  font-family: "Outfit", sans-serif;
}

.information h4 {
  font-size: clamp(16px, 4vw, 24px);
  font-family: "Outfit", sans-serif;
}

.mer-information {
  width: clamp(250px, 80%, 920px);
  display: flex;
  margin: 3vh 5vh;
  justify-content: space-between;
}

.mer-information h4 {
  font-size: clamp(16px, 4vw, 24px);
  font-family: "Outfit", sans-serif;
}

/* .mera-information {
  width: clamp(250px, 30%, 920px);
  display: flex;
  margin: 120vh 130vh;
  flex-direction: column;
  margin-top: -110vh;
  margin-bottom: 50vh;

} */

.mera-information h4 {
  font-size: clamp(16px, 4vw, 24px);
  font-family: "Outfit", sans-serif;
  margin-top: 1.5vh;
}

.mera-information p {
  font-size: clamp(12px, 3vw, 20px);
  font-family: "Outfit", sans-serif;
  line-height: 2.2em;
}
@media screen and (min-width: 950px) {
  .mera-information {
    width: clamp(250px, 30%, 920px);
    display: flex;
    margin: 120vh 133vh;
    flex-direction: column;
    margin-top: -100vh;
    margin-bottom: 40vh;
  }
}
</style>
