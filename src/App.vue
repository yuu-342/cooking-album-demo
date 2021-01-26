<template>
  <div id="app">
    <div class="container">
      <div class="title-area">
        <h1>あなたの料理記録アルバム</h1>
      </div>
      <ul class="item-list">
        <li v-for="(item, index) in cookingRecords" v-bind:key="index" class="item">
          <CookingRecord :imageUrl="item.image_url" :comment="item.comment" :recipeType="item.recipe_type" :recordedAt="item.recorded_at"/>
        </li>
        <infinite-loading @infinite="infiniteHandler">
          <span slot="no-more"></span>
          <span slot="no-results">
            <p style="color:white;">まだ記録がないようです。。<br>記録をしてあなただけのアルバムをつくりましょう！</p>
          </span>
        </infinite-loading>
      </ul>
    </div>
  </div>
</template>

<script>
import 'normalize.css';
import axios from 'axios';
import InfiniteLoading from 'vue-infinite-loading'
import CookingRecord from './components/CookingRecord';

export default {
  name: 'App',
  components: { CookingRecord, InfiniteLoading },
  data() {
    return {
      offset: 0,
      cookingRecords: [],
    }
  },
  async created() {
    const res = await this.fetchCookingRecords();
    this.cookingRecords = res;
  },
  methods: {
    fetchCookingRecords(){
      return axios.get('/cooking_records', {
        params: {
          offset: this.offset, 
          limit: 9,
        }
      }).then(res => {
        return res.data['cooking_records'];
      }).catch(error => {
        // 時間がないのでいったん雑にエラーを返してしまっている
        return error;
      })
    },
    async infiniteHandler($state){
      const res = await this.fetchCookingRecords();
      if(res.length){
        this.offset += 9;
        this.cookingRecords = this.cookingRecords.concat(res);
        $state.loaded();
      } else {
        $state.complete()
      }
    }
  }
}
</script>

<style>
#app {
  font-family: "Yu Gothic Medium", "游ゴシック Medium", YuGothic, "游ゴシック体", "メイリオ", sans-serif;
}

@media screen and (min-width: 767px) {
  .container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    width: 100vw;
    background-image: url('assets/images/cork.jpg');
    font-size: 18px;
  }

  .title-area {
    width: 100%;
    background-color: rgba(255, 255, 255, 0.7);
    text-align: center;
  }

  .title-area h1 {
    margin: 2rem 0;
    font-size: 2rem;
  }

  .item-list {
    padding: 0;
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
  }

  .item {
    margin: 2rem;
    list-style: none;
  }
}

@media screen and (max-width: 766px) {
  .container {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    width: 100vw;
    background-image: url('assets/images/cork.jpg');
    font-size: 16px;
  }

  .title-area {
    width: 100%;
    background-color: rgba(255, 255, 255, 0.7);
    text-align: center;
  }

  .title-area h1 {
    margin: 1.5rem 0;
    font-size: 1.4rem;
  }

  .item-list {
    padding: 0;
  }

  .item {
    margin: 2rem;
    list-style: none;
  }
}

</style>
