<template>
  <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 gap-4">
    <div v-for="pokemon in pokemonList" :key="pokemon.name" :class="pokemonCardClass(pokemon.types)" class="relative rounded-lg overflow-hidden border border-gray-200 hover:shadow-lg transform hover:scale-105 transition duration-300 ease-in-out">
      <div class="p-4">
        <img :src="pokemon.sprites.front_default" :alt="pokemon.name" class="mx-auto" style="max-width: 120px;">
        <p class="text-center font-bold mt-2">{{ pokemon.name }}</p>
        <div class="grid grid-cols-2 gap-2 mt-4">
          <div v-for="(stat, index) in pokemon.stats" :key="index" class="text-center">
            <p class="text-xs text-gray-500">{{ stat.stat.name.replace('-', ' ') }}</p>
            <p class="font-semibold">{{ stat.base_stat }}</p>
          </div>
        </div>
      </div>
      <div class="absolute inset-0 rounded-lg border-2 border-transparent animate-pulse"></div> <!-- Glowing effect -->
      <div class="p-2 text-center relative z-10">
        <p class="text-xs italic">Type: {{ pokemon.types.map(type => type.type.name).join(', ') }}</p>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const pokemonList = ref([]);

const fetchPokemonData = async () => {
  try {
    const response = await fetch('https://pokeapi.co/api/v2/pokemon?limit=20');
    const data = await response.json();
    const pokemonDetails = await Promise.all(data.results.map(async (result: any) => {
      const response = await fetch(result.url);
      return response.json();
    }));
    pokemonList.value = pokemonDetails;
  } catch (error) {
    console.error('Error fetching Pokémon data:', error);
  }
};

onMounted(() => {
  fetchPokemonData();
});

const pokemonCardClass = (types: any[]) => {
  if (types.length > 0) {
    const primaryType = types[0].type.name;
    switch (primaryType) {
      case 'grass':
        return 'bg-green-400 hover:bg-green-500';
      case 'fire':
        return 'bg-red-400 hover:bg-red-500';
      case 'water':
        return 'bg-blue-400 hover:bg-blue-500';
      case 'electric':
        return 'bg-yellow-400 hover:bg-yellow-500';
      case 'poison':
        return 'bg-purple-400 hover:bg-purple-500';
      case 'bug':
        return 'bg-green-400 hover:bg-green-500';
      case 'normal':
        return 'bg-gray-400 hover:bg-gray-500';
      case 'flying':
        return 'bg-blue-400 hover:bg-blue-500';
      case 'fighting':
        return 'bg-red-400 hover:bg-red-500';
      case 'psychic':
        return 'bg-purple-400 hover:bg-purple-500';
      case 'ground':
        return 'bg-yellow-400 hover:bg-yellow-500';
      case 'rock':
        return 'bg-gray-400 hover:bg-gray-500';
      case 'ghost':
        return 'bg-indigo-400 hover:bg-indigo-500';
      case 'ice':
        return 'bg-blue-400 hover:bg-blue-500';
      case 'dragon':
        return 'bg-indigo-400 hover:bg-indigo-500';
      case 'dark':
        return 'bg-gray-400 hover:bg-gray-500';
      case 'steel':
        return 'bg-gray-400 hover:bg-gray-500';
      case 'fairy':
        return 'bg-pink-400 hover:bg-pink-500';
      default:
        return 'bg-gray-400 hover:bg-gray-500';
    }
  } else {
    return 'bg-gray-400 hover:bg-gray-500';
  }
};
</script>

<style scoped>
.grid {
  padding: 20px;
}

.rounded-lg {
  border-radius: 12px;
}

.shadow-md {
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

.border {
  border-width: 1px;
}

.border-gray-200 {
  border-color: #e5e7eb;
}

.text-center {
  text-align: center;
}

.text-xs {
  font-size: 0.75rem;
}

.text-sm {
  font-size: 0.875rem;
}

.font-bold {
  font-weight: 700;
}

.font-semibold {
  font-weight: 600;
}

.text-gray-500 {
  color: #6b7280;
}

.text-xs.italic {
  font-size: 0.75rem;
  font-style: italic;
}

/* Background colors based on Pokémon types */
.bg-green-400 {
  background-color: #34d399; /* Darker Green */
}

.bg-green-500 {
  background-color: #10b981; /* Hover Green */
}

.bg-red-400 {
  background-color: #ef4444; /* Darker Red */
}

.bg-red-500 {
  background-color: #f87171; /* Hover Red */
}

.bg-blue-400 {
  background-color: #3b82f6; /* Darker Blue */
}

.bg-blue-500 {
  background-color: #60a5fa; /* Hover Blue */
}

.bg-yellow-400 {
  background-color: #fbbf24; /* Darker Yellow */
}

.bg-yellow-500 {
  background-color: #facc15; /* Hover Yellow */
}

.bg-purple-400 {
  background-color: #8b5cf6; /* Darker Purple */
}

.bg-purple-500 {
  background-color: #a855f7; /* Hover Purple */
}

.bg-indigo-400 {
  background-color: #6366f1; /* Darker Indigo */
}

.bg-indigo-500 {
  background-color: #7c3aed; /* Hover Indigo */
}

.bg-gray-400 {
  background-color: #9ca3af; /* Darker Gray */
}

.bg-gray-500 {
  background-color: #6b7280; /* Hover Gray */
}

.bg-pink-400 {
  background-color: #f472b6; /* Darker Pink */
}

.bg-pink-500 {
  background-color: #ec4899; /* Hover Pink */
}

/* Glowing effect animation */
@keyframes pulse {
  0%, 100% {
    border-color: rgba(255, 255, 255, 0.8); /* White */
    box-shadow: 0 0 0 0px rgba(255, 255, 255, 0.8); /* White glow */
  }
  50% {
    border-color: rgba(255, 255, 255, 0.4); /* Lighter White */
    box-shadow: 0 0 0 10px rgba(255, 255, 255, 0); /* Lighter White glow */
  }
}

.relative {
  animation: pulse 2s infinite; /* Apply glow animation */
}
</style>
