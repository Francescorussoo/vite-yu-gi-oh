<template>
    <div class="card">
      <img :src="card.card_images[0].image_url" alt="card name" />
      <div class="card-info">
        <h3>{{ card.name }}</h3>
        <p>{{ card.type }}</p>
      </div>
    </div>
  </template>
  
  <script setup>
  const props = defineProps({
    card: Object
  });
  </script>
  
  <style>
  .card {
    background-color: #fff;
    border: 1px solid #ddd;
    border-radius: 5px;
    width: 150px;
    text-align: center;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  .card img {
    width: 100%;
    height: auto;
    border-bottom: 1px solid #ddd;
  }
  
  .card-info {
    padding: 10px;
  }
  </style>
  