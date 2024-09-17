<template>
  <div>
    <select v-model="selectedCategory" @change="fetchCards">
      <option value="Alien">Alien</option>
      <!-- Altre opzioni qui -->
    </select>
    <div v-if="cards.length > 0">
      <div class="card-container">
        <CardItem v-for="card in cards" :key="card.id" :card="card" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import CardItem from './CardItem.vue';

const cards = ref([]);
const selectedCategory = ref('Alien');

const fetchCards = async () => {
  try {
    const response = await fetch(`https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0&archetype=${selectedCategory.value}`);
    if (!response.ok) {
      throw new Error(`HTTP errore! Stato: ${response.status}`);
    }
    const data = await response.json();
    cards.value = data.data;
  } catch (error) {
    console.error("Errore carte:", error.message);
  }
};

fetchCards();
</script>

<style>
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  justify-content: center;
  padding: 10px;
}
</style>
