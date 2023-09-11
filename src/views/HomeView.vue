<template>
  <v-container>
        <v-row>
          <v-col
            v-for="food in foods"
            :key="food.title"
            cols="4"
          >
            <v-card height="200">
              <div class="food-image">
                <img :src="food.image" alt="Food" />
              </div>
              {{food.title}}
            </v-card>
          </v-col>
        </v-row>
      </v-container>
</template>

<script>
  //import HelloWorld from '../components/HelloWorld'
  import axios from 'axios'

  export default {
    name: 'Home',

    data() {
      return{
        foods: []
      }
    },

    created() {
      this.getData();
    },

    methods: {
      getData(){
        let api = "https://the-cocktail-db3.p.rapidapi.com/"
        this.axios.get(api, {
          headers: {
            'X-RapidAPI-Key': '1f8770d103mshf8d79491f0ae610p1b6a37jsna005cb0020cc',
            'X-RapidAPI-Host': 'the-cocktail-db3.p.rapidapi.com'
          }
        }).then((response) => {
          console.log(response.data)
          this.foods = response.data;
        })
      }
    }

    //components: {
    //  HelloWorld,
    //},
  }
</script>

<style scoped>
.food-image {
  display: flex; /* Omogućava korištenje fleksibilnog modela */
  justify-content: center; /* Poravnava horizontalno na sredinu */
  align-items: center; /* Poravnava vertikalno na sredinu */
  height: 150px; /* Postavlja željenu visinu za karticu */
}
.food-image img {
  max-width: 100%;
  max-height: 130px; /* Postavlja željenu maksimalnu visinu */
  object-fit: cover; /* Omogućava da se slika pravilno skalira */

}
</style>
