<template>
    <div class="favorite-card" ref="cardRef">
        <div class="card-image-container">
            <img class="card-image" :src="image" alt="Title" />
            <button v-if="showRemoveButton" @click="handleRemove" class="remove-button">
                <img class="remove-icon" :src="removeIcon" alt="Remove" />
            </button>
        </div>
        <div class="card-content">
            <h2 class="card-title">{{ title }}</h2>
            <p class="card-description">{{ description }}</p>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'

const props = defineProps({
    image: {
        type: String,
        required: true,
        default: 'https://via.placeholder.com/400x500/444444/FFFFFF?text=No+Image',
    },
    title: {
        type: String,
        required: true,
        default: 'Untitled',
    },
    description: {
        type: String,
        required: true,
        default: 'No description available',
    },
    showRemoveButton: {
        type: Boolean,
        default: true,
    },
    removeIcon: {
        type: String,
        default:
            'data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMjQiIGhlaWdodD0iMjQiIHZpZXdCb3g9IjAgMCAyNCAyNCIgZmlsbD0ibm9uZSIgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIj4KPHBhdGggZD0iTTE4IDZMNiAxOCIgc3Ryb2tlPSJ3aGl0ZSIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiLz4KPHBhdGggZD0iTTYgNkwxOCAxOCIgc3Ryb2tlPSJ3aGl0ZSIgc3Ryb2tlLXdpZHRoPSIyIiBzdHJva2UtbGluZWNhcD0icm91bmQiIHN0cm9rZS1saW5lam9pbj0icm91bmQiLz4KPC9zdmc+',
    },
    animateOnMount: {
        type: Boolean,
        default: false,
    },
})

const emit = defineEmits(['remove', 'click'])

const cardRef = ref(null)

const handleRemove = () => {
    emit('remove')
}

const handleCardClick = () => {
    emit('click')
}

onMounted(() => {
    if (props.animateOnMount && cardRef.value) {
        gsap.set(cardRef.value, {
            opacity: 0,
            y: 100,
            scale: 0.95,
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
                        observer.unobserve(entry.target)
                    }
                })
            },
            { threshold: 0.2 },
        )

        observer.observe(cardRef.value)
    }
})
</script>

<style scoped>
.favorite-card {
    background-color: #1a1a1a;
    overflow: hidden;
    transition:
        box-shadow 0.4s ease,
        transform 0.3s ease;
    will-change: transform, box-shadow;
    cursor: pointer;
}

.favorite-card:hover {
    transform: translateY(-8px);
    box-shadow:
        0 0 8px 4px rgba(255, 255, 255, 0.069),
        0 0 20px 10px rgba(255, 255, 255, 0.071),
        0 0 30px 15px rgba(255, 255, 255, 0.065);
}

.card-image-container {
    position: relative;
    height: 0;
    padding-top: 133%;
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
    background-color: rgb(110, 110, 110);
    color: white;
    border: none;
    width: 60px;
    height: 60px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    transition: background-color 0.2s;
}

.remove-icon {
    width: 30px;
    height: 30px;
}

.remove-button:hover {
    background-color: rgba(255, 0, 0, 1);
}

.card-content {
    padding: 1rem;
    background-color: #3c3c3c;
}

.card-title {
    font-size: 1.25rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
    font-family: 'Oswald', 'sans-serif';
    color: white;
}

.card-description {
    font-size: 0.9rem;
    color: #aaa;
    line-height: 1.4;
    margin: 0;
}

@media (max-width: 1024px) {
    .card-title {
        font-size: 1.1rem;
    }

    .card-description {
        font-size: 0.85rem;
    }
}

@media (min-width: 1024px) {
    .remove-button {
        width: 40px;
        height: 40px;
    }

    .remove-icon {
        width: 12px;
        height: 12px;
    }
}
</style>
