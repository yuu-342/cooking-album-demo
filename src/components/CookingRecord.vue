<template>
  <div :class="['cooking-record', recipeType]">
    <p class="recorded-time">{{ recordedAt | moment() }}</p>
    <div class="content">
      <img :src="imageUrl" class="record-image"/>
      <p>{{ comment }}</p>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: 'CookingRecord',
  props: {
    imageUrl: String,
    comment: String,
    recipeType: String,
    recordedAt: String
  },
  filters: {
    moment(value) {
      return moment(value).format('YYYY.MM.DD');
    }
  }
}
</script>

<style scoped>
@font-face {
  font-family: 'karakaze';
  src: url('../assets/fonts/ShigotoMemogaki-Regular-1-01.ttf');
}
.cooking-record {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  justify-content: center;
  border-radius: 0.5rem;
  box-shadow: 2px 2px 3px;
}

.content {
  width: 80%;
  margin: 0 auto 1rem;
  padding: 0 1rem;
  font-size: 1.2rem;
  background-color: rgba(255, 255, 255,0.5);
  border-radius: 0.5rem;
}

.recorded-time {
  margin-left: 1rem;
}
.record-image {
  width: 100%;
  margin: 1rem 0 0;
}

.main_dish {
  background-image: url('../assets/images/main_dish.png');
}

.side_dish {
  background-image: url('../assets/images/side_dish.png');
}

.soup {
  background-image: url('../assets/images/soup.png');
}
</style>