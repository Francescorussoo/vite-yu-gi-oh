<template>
  <div>
    <select v-model="selectedCategory" @change="fetchCards">
      <option value="" disabled>Seleziona un Archetipo</option>
      <option v-for="archetype in archetypes" :key="archetype" :value="archetype">
        {{ archetype }}
      </option>
    </select>

    <div v-if="cards.length > 0" class="card-container">     
      <CardItem v-for="card in cards" :key="card.id" :card="card" />
    </div>
    <div v-else>
      <p>Nessuna carta disponibile per questo archetipo.</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import CardItem from './CardItem.vue';

const cards = ref([]);
const archetypes = ref([]);
const selectedCategory = ref('');


const fetchArchetypes = async () => {
  try {
    const response = await fetch('https://db.ygoprodeck.com/api/v7/archetypes.php');
    if (!response.ok) {
      throw new Error(`Errore HTTP! Stato: ${response.status}`);
    }
    const data = await response.json();
    archetypes.value = data.map(item => item.archetype_name);
  } catch (error) {
    console.error("Errore nel caricamento degli archetipi:", error.message);
  }
};

const fetchCards = async () => {
  if (!selectedCategory.value) return;
  try {
    const response = await fetch(`https://db.ygoprodeck.com/api/v7/cardinfo.php?archetype=${encodeURIComponent(selectedCategory.value)}&num=20&offset=0`);
    if (!response.ok) {
      throw new Error(`Errore HTTP! Stato: ${response.status}`);
    }
    const data = await response.json();
    console.log("Risposta completa dall'API:", data);

    if (Array.isArray(data)) {
      cards.value = data;
    } else if (data.data && Array.isArray(data.data)) {
      cards.value = data.data;
    } else {
      console.error("Formato dei dati inaspettato:", data);
    }

  } catch (error) {
    console.error("Errore nel caricamento delle carte:", error.message);
  }
};

onMounted(() => {
  fetchArchetypes();
});
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
