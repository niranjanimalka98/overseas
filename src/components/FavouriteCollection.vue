<template>
  <section>
<div class="favorites-container px-5 pb-5">
    <div class="favorites-header">
      <h1 class="favorites-title">Collect your favourites</h1>
      <div class="search-container">
        <input 
          type="text" 
          v-model="searchQuery" 
          placeholder="Search title and add to grid" 
          @keyup.enter="searchAndAdd"
          class="search-input"
        />
        <img src="../assets/Icons/Search White.svg" class="search-icon" />
      </div>
    </div>
    
    <div class="divider"></div>
    
    <div class="favorites-grid">
      <div v-for="(item, index) in favorites" :key="index" class="favorite-card">
        <div class="card-image-container">
          <img :src="item.image" :alt="item.title" class="card-image" />
          <button @click="removeItem(index)" class="remove-button">
            <img src="../assets/Icons/Close White.svg" alt="Remove" />
          </button>
        </div>
        <div class="card-content">
          <h2 class="card-title">{{ item.title }}</h2>
          <p class="card-description">{{ item.description }}</p>
        </div>
      </div>
    </div>
  </div>
</section>
</template>

<script setup>
import { ref } from 'vue';
import batmanImage from '../assets/Images/Batman.jpg';
import wildWildWestImage from '../assets/Images/WildWest.jpg';
import spidermanImage from '../assets/Images/Spiderman.jpg';

// Sample data for initial favorites
const favorites = ref([
  {
    title: 'Batman Returns',
    description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
    image: batmanImage
  },
  {
    title: 'Wild Wild West',
    description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
    image: wildWildWestImage
  },
  {
    title: 'The Amazing Spiderman',
    description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
    image: spidermanImage
  }
]);

const searchQuery = ref('');

// Sample movie database for search functionality
const movieDatabase = [
  {
    title: 'Inception',
    description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
    image: 'https://via.placeholder.com/400x500/0000FF/FFFFFF?text=Inception'
  },
  {
    title: 'The Dark Knight',
    description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
    image: 'https://via.placeholder.com/400x500/333333/FFFFFF?text=Dark+Knight'
  },
  {
    title: 'Pulp Fiction',
    description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
    image: 'https://via.placeholder.com/400x500/FFFF00/000000?text=Pulp+Fiction'
  }
];

// Function to search and add a movie to favorites
const searchAndAdd = () => {
  if (!searchQuery.value.trim()) return;
  
  // Simple search implementation
  const foundMovie = movieDatabase.find(movie => 
    movie.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  );
  
  if (foundMovie) {
    // Check if movie already exists in favorites
    const exists = favorites.value.some(item => item.title === foundMovie.title);
    if (!exists) {
      favorites.value.push({ ...foundMovie });
    }
  } else {
    // If not found, create a new entry with the search query
    favorites.value.push({
      title: searchQuery.value,
      description: 'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
      image: `https://via.placeholder.com/400x500/444444/FFFFFF?text=${encodeURIComponent(searchQuery.value)}`
    });
  }
  
  // Clear search input
  searchQuery.value = '';
};

// Function to remove an item from favorites
const removeItem = (index) => {
  favorites.value.splice(index, 1);
};
</script>

<style scoped>
.favorites-container {
  background-color: #121212;
  color: white;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.favorites-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.favorites-title {
  font-size: 2rem;
  font-weight: 600;
}

.search-container {
  position: relative;
  width: 360px;
}

.search-input {
  width: 100%;
  padding: 0.75rem 1rem 0.75rem 2.5rem;
  border: 1px solid #333;
  border-radius: 4px;
  background-color: transparent;
  color: white;
  font-size: 0.9rem;
}

.search-icon {
  position: absolute;
  left: 0.75rem;
  top: 50%;
  transform: translateY(-50%);
  color: #888;
}

.divider {
  height: 1px;
  background-color: #333;
  margin: 1rem 0 2rem;
}

.favorites-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(500px, 1fr));
  gap: 1.5rem;
}

.favorite-card {
  background-color: #1a1a1a;
  border-radius: 4px;
  overflow: hidden;
}

.card-image-container {
  position: relative;
  height: 0;
  padding-top: 133%; /* 4:3 aspect ratio */
  overflow: hidden;
}

.card-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.remove-button {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background-color: rgba(110, 110, 110, 0.5);
  color: white;
  border: none;
  border-radius: 4px;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background-color 0.2s;
}

.remove-button:hover {
  background-color: rgba(255, 0, 0, 0.7);
}

.card-content {
  padding: 1rem;
}

.card-title {
  font-size: 1.25rem;
  font-weight: 600;
  margin-bottom: 0.5rem;
}

.card-description {
  font-size: 0.9rem;
  color: #aaa;
  line-height: 1.4;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .favorites-header {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .search-container {
    width: 100%;
    margin-top: 1rem;
  }
  
  .favorites-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  }
}
</style>