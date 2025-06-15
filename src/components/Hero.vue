<template>
    <section class="hero">
        <img src="../assets/Images/HeaderImage.jpg" alt="Theater Seats" class="hero-image" />

        <div class="row px-3 px-3-sm px-5-md px-5-lg px-5-xl py-4"
            style="background-color: black; position: relative; margin-top: -5px">
            <div class="col-12 col-md-9 col-lg-5">
                <h1 class="hero-title" ref="heroTitle">MOVIE LIBRARY</h1>
                <p class="mb-3" ref="heroSubTitle">
                    Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor
                    invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua.
                </p>
            </div>
        </div>
    </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { gsap } from 'gsap'

const heroTitle = ref(null)
const heroSubTitle = ref(null)

onMounted(() => {
    const tl = gsap.timeline()

    tl.from(heroTitle.value, {
        opacity: 0,
        y: 50,
        duration: 1,
        ease: 'power3.out',
    }).from(
        heroSubTitle.value,
        {
            opacity: 0,
            y: 30,
            duration: 1,
            ease: 'power3.out',
        },
        '-=0.5',
    )
})
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Inter', sans-serif;
    background-color: #000;
    color: #fff;
    line-height: 1.6;
}

.container {
    width: 90%;
    max-width: 1200px;
    margin: 0 auto;
}

a {
    text-decoration: none;
    color: #fff;
    transition: color 0.3s ease;
}

a:hover {
    color: #e6b800;
}

ul {
    list-style: none;
}

header {
    background-color: rgba(0, 0, 0, 0.9);
    position: fixed;
    width: 100%;
    z-index: 1000;
    padding: 20px 0;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

header .container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.hero-title {
    color: #ffffff;
}

.logo {
    display: flex;
    align-items: center;
}

.logo img {
    height: 40px;
    margin-right: 10px;
}

.logo span {
    font-size: 24px;
    font-weight: 700;
    color: #fff;
}

nav {
    display: flex;
    align-items: center;
}

.desktop-menu {
    display: flex;
}

.desktop-menu li {
    margin-left: 30px;
}

.desktop-menu a {
    font-size: 14px;
    font-weight: 600;
    letter-spacing: 1px;
    text-transform: uppercase;
    position: relative;
}

.desktop-menu a.active {
    color: #e6b800;
}

.desktop-menu a.active::after {
    content: '';
    position: absolute;
    bottom: -5px;
    left: 0;
    width: 100%;
    height: 2px;
    background-color: #e6b800;
}

.hamburger {
    display: none;
    background: none;
    border: none;
    cursor: pointer;
    padding: 10px;
}

.hamburger span {
    display: block;
    width: 25px;
    height: 3px;
    background-color: #fff;
    margin: 5px 0;
    transition: all 0.3s ease;
}

.mobile-menu {
    display: none;
    position: fixed;
    top: 80px;
    left: 0;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.95);
    z-index: 999;
    padding: 20px 0;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    transform: translateY(-100%);
    transition: transform 0.3s ease;
}

.mobile-menu.active {
    transform: translateY(0);
    display: block;
}

.mobile-menu ul {
    padding: 0 20px;
}

.mobile-menu li {
    margin: 15px 0;
}

.mobile-menu a {
    font-size: 16px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.mobile-menu a.active {
    color: #e6b800;
}

.hero {
    position: relative;
}

.hero-image {
    width: 100%;
    height: 100vh;
    margin-top: -320px;
    position: relative;
    object-fit: cover;
    object-position: center;
}

.content {
    padding: 60px 0;
}

h1 {
    font-size: 42px;
    font-weight: 700;
    margin-bottom: 20px;
    text-transform: uppercase;
    letter-spacing: 2px;
}

p {
    font-size: 16px;
    max-width: 800px;
    margin-bottom: 40px;
    color: #ccc;
}

.movie-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    gap: 30px;
    margin-top: 40px;
}

.movie-card {
    background-color: #111;
    border-radius: 8px;
    overflow: hidden;
    transition: transform 0.3s ease;
}

.movie-card:hover {
    transform: translateY(-10px);
}

.movie-poster {
    width: 100%;
    height: 350px;
    object-fit: cover;
}

.movie-info {
    padding: 20px;
}

.movie-title {
    font-size: 18px;
    font-weight: 600;
    margin-bottom: 10px;
}

.movie-details {
    display: flex;
    justify-content: space-between;
    color: #aaa;
    font-size: 14px;
}

footer {
    background-color: #111;
    padding: 60px 0 20px;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
}

.footer-content {
    display: flex;
    justify-content: space-between;
    margin-bottom: 40px;
}

.footer-logo {
    display: flex;
    align-items: center;
    margin-bottom: 20px;
}

.footer-logo img {
    height: 30px;
    margin-right: 10px;
}

.footer-links {
    display: flex;
    gap: 60px;
}

.footer-column h3 {
    font-size: 18px;
    margin-bottom: 20px;
    color: #e6b800;
}

.footer-column ul li {
    margin-bottom: 10px;
}

.copyright {
    text-align: center;
    padding-top: 20px;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
    color: #666;
    font-size: 14px;
}

@media (max-width: 992px) {
    .desktop-menu {
        display: none;
    }

    .hamburger {
        display: block;
    }

    .footer-content {
        flex-direction: column;
    }

    .footer-links {
        flex-direction: column;
        gap: 30px;
    }
}

@media (max-width: 768px) {
    h1 {
        font-size: 32px;
    }
}

@media (max-width: 576px) {
    .container {
        width: 95%;
    }

    h1 {
        font-size: 28px;
    }

    p {
        font-size: 14px;
    }
}
</style>
