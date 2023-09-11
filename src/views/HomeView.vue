<template>
  <v-container>
    <v-text-field v-model="searchTerm" label="Pretraga koktela" @input="searchCocktails" />
    
    <v-row>
      <v-col
        v-for="cocktail in paginatedCocktails"
        :key="cocktail.title"
        cols="4"
      >
        <v-card height="200">
          <div class="cocktail-image">
            <img :src="cocktail.image" alt="Cocktail" />
          </div>
          <h3>{{ cocktail.title }}</h3>
        </v-card>
      </v-col>
    </v-row>
    
    <v-row>
      <v-col cols="12" class="text-center">
        <div class="button-container">
          <v-btn @click="currentPage--" :disabled="currentPage === 1">Prethodna</v-btn>
          <v-btn @click="currentPage++" :disabled="currentPage * cocktailsPerPage >= filteredCocktails.length">Sljedeća</v-btn>
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Home',

  data() {
    return {
      cocktails: [],
      currentPage: 1,
      cocktailsPerPage: 9,
      searchTerm: ''
    };
  },

  created() {
    this.getData();
  },

  computed: {
    filteredCocktails() {
      return this.cocktails.filter(cocktail => {
        const startsWithSearchTerm = cocktail.title.toLowerCase().startsWith(this.searchTerm.toLowerCase());
        const includesSearchTerm = cocktail.title.toLowerCase().includes(this.searchTerm.toLowerCase());
        return startsWithSearchTerm || includesSearchTerm;
      });
    },
    paginatedCocktails() {
      const startIndex = (this.currentPage - 1) * this.cocktailsPerPage;
      const endIndex = startIndex + this.cocktailsPerPage;
      return this.filteredCocktails.slice(startIndex, endIndex);
    }
  },

  methods: {
    getData() {
      let api = "https://the-cocktail-db3.p.rapidapi.com/";
      axios
        .get(api, {
          headers: {
            'X-RapidAPI-Key': '1f8770d103mshf8d79491f0ae610p1b6a37jsna005cb0020cc',
            'X-RapidAPI-Host': 'the-cocktail-db3.p.rapidapi.com'
          }
        })
        .then((response) => {
          console.log(response.data);
          this.cocktails = response.data;
        });
    },
    searchCocktails() {
      this.currentPage = 1;
    }
  }
};
</script>

<style scoped>
.cocktail-image {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 150px;
}
.cocktail-image img {
  max-width: 100%;
  max-height: 130px;
  object-fit: cover;
}
.button-container {
  display: flex;
  justify-content: space-between;
}
</style>
