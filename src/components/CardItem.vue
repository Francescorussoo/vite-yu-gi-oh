<template>
  <div class="card">
    <img
      v-if="card.card_images && card.card_images.length > 0"
      :src="card.card_images[0].image_url"
      :alt="card.name"
      class="card-image"
    />
    <div class="card-info">
      <h3>{{ card.name }}</h3>
      <p>{{ card.type }}</p>
      <p v-if="card.desc">{{ card.desc }}</p>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  card: {
    type: Object,
    required: true,
  },
});
</script>

<style>
.card {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  width: 180px;
  text-align: center;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  margin: 10px;
  overflow: hidden;
}

.card-image {
  width: 100%;
  height: auto;
  border-bottom: 1px solid #ddd;
}

.card-info {
  padding: 10px;
}
</style>
