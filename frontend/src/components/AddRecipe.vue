<script>
  import axios from 'axios'
  export default {
    created() {
      axios.get('http://localhost:3000/recipe').then((result) => {
        this.recipes = result.data
        // console.log(result.data)
      })
    },
    data() {
      return {
        recipes: null,
        name: '',
        instructions: '',
        time: '',
        url: '',
        category: '1',
        selectedRecId: '',
        recipeMessage: false,
        categoryMessage: false
      }
    },
    methods: {
      submitRecipe() {
        axios
          .post('http://localhost:3000/recipe', {
            recipeName: this.name,
            recipeInstructions: this.instructions,
            recipeTime: this.time,
            recipeImage: this.url
          })
          .then((response) => {
            this.response = JSON.stringify(response)
          })
        setTimeout(() => {
          location.reload()
        }, 1000) // Laddar om sidan efter 1 sekund
      },
      connectCategory() {
        axios
          .post('http://localhost:3000/recipeCategory', {
            recipeCategoryCatId: this.category,
            recipeCategoryRecId: this.selectedRecId
          })
          .then((response) => {
            this.response = JSON.stringify(response)
          })
      },
      submitRecipeMessage() {
        this.recipeMessage = true
      },
      submitCategoryMessage() {
        this.categoryMessage = true
      }
    }
  }
</script>

<template>
  <h1>Lägg till ett nytt recept</h1>

  <form @submit.prevent="submitRecipe">
    <div class="flex">
      <label for="name">Namn:</label>
      <input id="name" type="text" v-model="name" required />

      <label for="instructions">Instruktioner:</label>
      <textarea
        id="instructions"
        type="text"
        v-model="instructions"
        required
        rows="5"
        cols="40"
      />

      <label for="time">Tillagningstid:</label>
      <input id="time" type="text" v-model="time" required />

      <label for="url">Bild Url:</label>
      <input id="url" type="text" v-model="url" required />

      <button type="submit" @click="submitRecipeMessage()">
        Skicka ditt recept!
      </button>
    </div>
  </form>
  <h4 v-if="recipeMessage">Ditt recept har skapats!</h4>

  <br />
  <h4>Koppla ditt recept med en kategori</h4>
  <form @submit.prevent="connectCategory">
    <div class="flex">
      <label>Välj ditt recept</label>
      <select v-model="selectedRecId">
        <option
          v-for="recipe in recipes"
          :key="recipe.recipeId"
          :value="recipe.recipeId"
        >
          {{ recipe.recipeName }}
        </option>
      </select>
    </div>

    <div class="flex">
      <label>Välj kategori</label>
      <label><input type="radio" v-model="category" value="1" />Kött</label>
      <label><input type="radio" v-model="category" value="2" />Fisk</label>
      <label><input type="radio" v-model="category" value="3" />Kyckling</label>
      <label><input type="radio" v-model="category" value="4" />Vegan</label>
      <label
        ><input type="radio" v-model="category" value="5" />Vegetariskt</label
      >
      <button type="submit" @click="submitCategoryMessage()">
        Koppa ditt recept med en kategori!
      </button>
    </div>
  </form>
  <h4 v-if="categoryMessage">Du har nu kopplat ditt recept med en kategori!</h4>
</template>

<style scoped>
  h1 {
    margin-top: 30px;
    margin-bottom: 30px;
    margin-left: 5px;
    text-align: center;
  }

  h4 {
    margin-left: 5px;
    text-align: center;
  }
  .flex {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-bottom: 25px;
  }

  .flex input[type='text'] {
    width: 20%;
  }

  button {
    width: 20%;
    margin-top: 5px;
    padding: 5px;
    border-radius: 10px;
  }

  textarea {
    width: 20%;
    height: 100px;
  }

  form {
    margin-left: 5px;
  }
</style>
