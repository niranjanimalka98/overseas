<template>
    <section>
        <div class="favorites-container py-4 px-3 px-5-md px-5-lg">
            <div class="favorites-header row">
                <div class="col-12 col-md-7 col-lg-9 favorites-title-container">
                    <h1 class="favorites-title" ref="favoritesTitleRef">Collect your favourites</h1>
                </div>

                <div class="search-container col-12 col-md-5 col-lg-3" ref="favoritesSearchRef">
                    <input type="text" v-model="searchQuery" placeholder="Search title and add to grid"
                        @keyup.enter="searchAndAdd" class="search-input" />
                    <img src="../assets/Icons/Search White.svg" alt="Search Icon" class="search-icon" />
                </div>
            </div>

            <div class="divider" ref="favoriesLineRef"></div>

            <div class="favorites-grid">
                <MovieCard v-for="(item, index) in favorites" :key="`${item.title}-${index}`" :image="item.image"
                    :title="item.title" :description="item.description" :animate-on-mount="true"
                    @remove="removeItem(index)" @click="handleCardClick(item, index)" />
            </div>
        </div>
    </section>
</template>

<script setup>
import MovieCard from './UI/MovieCard.vue'
import batmanImage from '../assets/Images/Batman.jpg'
import wildWildWestImage from '../assets/Images/WildWest.jpg'
import spidermanImage from '../assets/Images/Spiderman.jpg'

import { onMounted, ref } from 'vue'
import { gsap } from 'gsap'

const favoritesTitleRef = ref(null)
const favoritesSearchRef = ref(null)
const favoriesLineRef = ref(null)

onMounted(() => {
    gsap.set([favoritesTitleRef.value, favoritesSearchRef.value], {
        opacity: 0,
        y: 100,
        scale: 0.95,
    })

    gsap.set(favoriesLineRef.value, {
        scaleX: 0,
        transformOrigin: 'left center',
    })

    const observer = new IntersectionObserver(
        (entries) => {
            entries.forEach((entry) => {
                if (entry.isIntersecting) {
                    gsap.to(entry.target, {
                        opacity: 1,
                        y: 0,
                        scale: 1,
                        duration: 1.5,
                        ease: 'power4.out',
                    })

                    if (entry.target === favoriesLineRef.value) {
                        gsap.to(favoriesLineRef.value, {
                            scaleX: 1,
                            duration: 1.2,
                            ease: 'power2.out',
                        })
                    }

                    observer.unobserve(entry.target)
                }
            })
        },
        { threshold: 0.2 },
    )

    observer.observe(favoritesTitleRef.value)
    observer.observe(favoritesSearchRef.value)
    observer.observe(favoriesLineRef.value)
})

const favorites = ref([
    {
        title: 'Batman Returns',
        description:
            'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
        image: batmanImage,
    },
    {
        title: 'Wild Wild West',
        description:
            'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
        image: wildWildWestImage,
    },
    {
        title: 'The Amazing Spiderman',
        description:
            'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
        image: spidermanImage,
    },
])

const searchQuery = ref('')

const movieDatabase = [
    {
        title: 'Inception',
        description:
            'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
        image: 'https://flhsnavigator.com/wp-content/uploads/2021/10/url-4.jpeg?w=691',
    },
    {
        title: 'The Dark Knight',
        description:
            'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
        image: 'https://musicart.xboxlive.com/7/abb02f00-0000-0000-0000-000000000002/504/image.jpg',
    },
    {
        title: 'Avengers',
        description:
            'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
        image: 'https://m.media-amazon.com/images/M/MV5BNGE0YTVjNzUtNzJjOS00NGNlLTgxMzctZTY4YTE1Y2Y1ZTU4XkEyXkFqcGc@._V1_FMjpg_UX1000_.jpg',
    },
]

const searchAndAdd = () => {
    if (!searchQuery.value.trim()) return

    const foundMovie = movieDatabase.find((movie) =>
        movie.title.toLowerCase().includes(searchQuery.value.toLowerCase()),
    )

    if (foundMovie) {
        const exists = favorites.value.some((item) => item.title === foundMovie.title)
        if (!exists) {
            favorites.value.push({ ...foundMovie })
        }
    } else {
        favorites.value.push({
            title: searchQuery.value,
            description:
                'Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut...',
            image: `https://via.placeholder.com/400x500/444444/FFFFFF?text=${encodeURIComponent(searchQuery.value)}`,
        })
    }

    searchQuery.value = ''
}

const removeItem = (index) => {
    favorites.value.splice(index, 1)
}

const handleCardClick = (item, index) => {
    console.log('Card clicked:', item, index)
}
</script>

<style scoped>
.favorites-container {
    background-color: #1d1d1d;
    color: white;
    font-family:
        -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans',
        'Helvetica Neue', sans-serif;
}

.favorites-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
}

.favorites-title {
    font-size: 1.7rem !important;
    font-weight: 600;
}

.search-container {
    position: relative;
    width: 360px;
    margin-top: 20px;
    padding: 0;
}

.search-input {
    width: 100%;
    padding: 0.75rem 1rem 0.75rem 2.5rem;
    border: 1px solid #fff;
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
    height: 2px;
    background-color: #ffffff;
    margin: 1rem 0 2rem;
}

.favorites-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(450px, 1fr));
    gap: 1.5rem;
}

.favorites-title-container {
    padding: 0;
}

@media (max-width: 768px) {
    .search-container {
        width: 100%;
    }

    .favorites-grid {
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    }
}

@media (max-width: 1024px) {
    .favorites-grid {
        grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    }
}

@media (min-width: 1200px) {
    .favorites-grid {
        grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
    }
}
</style>
