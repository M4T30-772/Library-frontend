<template>
  <div class="test" inline-block>
    <!-- za trazit uredi!! -->
    <div class="row">
      <div class="col-2"></div>
      <div class="col-6">
        <generirajCard
          v-for="card in filteredCards"
          :key="card.url"
          :info="card"
        />
      </div>
      <div class="col-2"></div>
    </div>
  </div>
</template>

<script>
import store from "./store.js";
import generirajCard from "./generirajCard.vue";

//let cards = [];

export default {
  name: "ListaKnjiga",
  data: function() {
    return {
      cards: [],
      store,
    };
  },
  //pokaze se cim otvorimo home
  mounted() {
    this.getPosts();
  },

  methods: {
    Posudba() {
      console.log("radim");
    },
    getPosts() {
      fetch("http://localhost:3000/knjige")
        .then((r) => {
          return r.json();
        })
        .then((data) => {
          console.log("podatci sa backenda", data);
          let data2 = data.map((element) => {
            return {
              id: element.id,
              Autor: element.Autor,
              Naziv: element.Naziv,
              Url: element.Url,
              Desc: element.Desc,
              Knjizevni_Rod: element.Knjizevni_Rod,
              Datum: element.Datum,
              Pdf: element.Pdf,
            };
          });
          this.cards = data2;
        });
    },
  },
  computed: {
    filteredCards() {
      //search bar
      let termin = this.store.searchTerm;
      let newCards = [];
      for (let card of this.cards) {
        if (card.Desc.indexOf(termin) >= 0) {
          console.log("prolaz");
          newCards.push(card);
        }
      }
      return newCards;
    },
  },
  components: {
    generirajCard,
  },
};
</script>

<style></style>
