<template>
  <div id="app">
    <div class="container">
      <ul class="item-list">
        <li v-for="(item, index) in cookingRecords" v-bind:key="index" class="item">
          <CookingRecord :imageUrl="item.image_url" :comment="item.comment" :recipeType="item.recipe_type" :recordedAt="item.recorded_at"/>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import 'normalize.css';
import axios from 'axios';
import CookingRecord from './components/CookingRecord';

export default {
  name: 'App',
  components: { CookingRecord },
  data() {
    return {
      cookingRecords: []
    }
  },
  created() {
    axios.get('/cooking_records').then(response => {
      console.log(response.data)
      this.cookingRecords = response.data['cooking_records'];
    })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

.container {
  display: flex;
  width: 100vw;
  background-image: url('assets/images/cork.jpg');
}

.item-list {
  padding: 0;
}

.item {
  margin: 2rem;
  list-style: none;
}
</style>
