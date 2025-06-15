<template>
    <header class="nav-navbar">
        <!-- Logo - Left side -->
        <div class="nav-logo ml-4 mr-4">
            <img src="../assets/Logos/Logo White.svg" alt="Logo" />
        </div>

        <!-- Right side container for nav links and hamburger -->
        <div class="nav-right-section">
            <!-- Desktop/Tablet Navigation - Show different amounts based on screen size -->
            <nav class="nav-nav-links desktop">
                <div v-for="(item, index) in visibleNavItems" :key="item.id" class="nav-item">
                    <a :href="item.href" @click="handleNavClick">{{ item.label }}</a>
                </div>
            </nav>

            <!-- Hamburger Menu - Always visible -->
            <button class="nav-hamburger" @click="toggleSidebar">
                <img src="../assets/Icons/Menu White.svg" alt="Menu" />
            </button>
        </div>

        <!-- Sidebar -->
        <div class="nav-sidebar" :class="{ open: isSidebarOpen, 'rtl-mode': isRTL }">
            <button class="nav-close-btn" @click="toggleSidebar">&times;</button>

            <!-- Desktop/Tablet: Show remaining items -->
            <div v-if="!isMobile" class="sidebar-section">
                <a v-for="item in hiddenNavItems" :key="`hidden-${item.id}`" :href="item.href" @click="toggleSidebar">
                    {{ item.label }}
                </a>
            </div>

            <!-- Mobile: Show all items -->
            <div v-else class="sidebar-section">
                <a v-for="item in navigationItems" :key="`mobile-${item.id}`" :href="item.href" @click="toggleSidebar">
                    {{ item.label }}
                </a>
            </div>
        </div>

        <!-- Overlay -->
        <div class="nav-overlay" v-if="isSidebarOpen" @click="toggleSidebar"></div>

        <!-- Add this debug info temporarily to see if RTL is detected -->
    </header>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'

// Navigation items as JSON data
const navigationItems = ref([
    { id: 1, label: 'HOME', href: '/' },
    { id: 2, label: 'OUR SCREENS', href: '/about' },
    { id: 3, label: 'SCHEDULE', href: '/services' },
    { id: 4, label: 'MOVIE LIBRARY', href: '/portfolio' },
    { id: 5, label: 'LOCATION & CONTACT', href: '/contact' },
    { id: 6, label: 'GALLERY', href: '/blog' },
])

const isSidebarOpen = ref(false)
const isMobile = ref(false)
const isTablet = ref(false)
const isDesktop = ref(false)

// Add this reactive variable and better RTL detection
const isRTL = ref(false)

// Improved RTL detection function
const checkRTLMode = () => {
    const rtlMode = localStorage.getItem('rtl-mode')
    isRTL.value = rtlMode === 'true'
}

// Responsive computed property for visible nav items
const visibleNavItems = computed(() => {
    if (isDesktop.value) {
        // Desktop: Show 5 items
        return navigationItems.value.slice(0, 5)
    } else if (isTablet.value) {
        // Tablet: Show 4 items
        return navigationItems.value.slice(0, 4)
    } else {
        // Mobile: Show no items (all in sidebar)
        return []
    }
})

// Remaining items for sidebar
const hiddenNavItems = computed(() => {
    if (isDesktop.value) {
        // Desktop: Show items 6+
        return navigationItems.value.slice(5)
    } else if (isTablet.value) {
        // Tablet: Show items 5+
        return navigationItems.value.slice(4)
    } else {
        // Mobile: All items shown in sidebar
        return []
    }
})

const toggleSidebar = () => {
    isSidebarOpen.value = !isSidebarOpen.value
}

const handleNavClick = () => {
    if (isSidebarOpen.value) {
        isSidebarOpen.value = false
    }
}

// Check screen size and set responsive flags
const checkScreenSize = () => {
    const width = window.innerWidth

    isMobile.value = width <= 768
    isTablet.value = width > 768 && width <= 1024
    isDesktop.value = width > 1024
}

// Listen for localStorage changes
const handleStorageChange = (e) => {
    if (e.key === 'rtl-mode') {
        checkRTLMode()
    }
}

// Add interval check for RTL changes
let rtlCheckInterval = null

onMounted(() => {
    checkScreenSize()
    checkRTLMode()

    // Check RTL mode every 500ms to catch changes
    rtlCheckInterval = setInterval(checkRTLMode, 500)

    window.addEventListener('resize', checkScreenSize)
    window.addEventListener('storage', handleStorageChange)
})

onUnmounted(() => {
    if (rtlCheckInterval) {
        clearInterval(rtlCheckInterval)
    }
    window.removeEventListener('resize', checkScreenSize)
    window.removeEventListener('storage', handleStorageChange)
})
</script>

<style scoped>
/* Top Navbar */
.nav-navbar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #000000;
    position: sticky;
    color: white;
    z-index: 1000;
    top: 0;
padding: 0px 17px;
}

/* Logo - Left side */
.nav-logo {
    padding: 20px 5px;
    flex-shrink: 0;
}

.nav-logo img {
    height: 40px;
}

/* Right section container */
.nav-right-section {
    display: flex;
    align-items: center;
    gap: 0;
}

.nav-nav-links {
    display: flex;
    align-items: center;
}

.nav-nav-links a {
    color: white;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.3s ease;
    font-size: 14px;
    letter-spacing: 0.5px;
}

.nav-item {
    padding: 20px 15px;
    transition: background-color 0.3s ease;
}

.nav-item:hover {
    background-color: #424242;
}

.nav-nav-links a:hover {
    text-decoration: underline;
}

/* Hamburger Menu - Always visible, aligned with nav */
.nav-hamburger {
    font-size: 1.5rem;
    background: none;
    color: white;
    border: none;
    cursor: pointer;
    width: 40px;
    height: 40px;
    margin-right: 20px;
    margin-left: 15px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background-color 0.3s ease;
    flex-shrink: 0;
}

.nav-hamburger:hover {
    background-color: #1d1d1d;
}

.nav-hamburger img {
    width: 24px;
    height: 24px;
}

.nav-sidebar {
    position: fixed;
    top: 0;
    right: -300px;
    left: auto;
    width: 300px;
    height: 100%;
    background-color: #000;
    color: white;
    padding: 2rem 1rem;
    display: flex;
    flex-direction: column;
    transition:
        right 0.3s ease-in-out,
        left 0.3s ease-in-out;
    z-index: 1100;
    overflow-y: auto;
}

.nav-sidebar.open {
    right: 0;
}

/* RTL mode - sidebar comes from left */
.nav-sidebar.rtl-mode {
    right: auto !important;
    left: -300px !important;
}

.nav-sidebar.rtl-mode.open {
    left: 0 !important;
    right: auto !important;
}

/* Mobile RTL fixes */
@media (max-width: 768px) {
    .nav-sidebar.rtl-mode {
        left: -280px !important;
        right: auto !important;
    }
}

@media (max-width: 480px) {
    .nav-sidebar.rtl-mode {
        left: -100% !important;
        right: auto !important;
    }
}

.sidebar-section {
    display: flex;
    flex-direction: column;
}

.nav-sidebar a {
    color: #bdc3c7;
    margin: 0.5rem 0;
    text-decoration: none;
    font-size: 1rem;
    padding: 0.75rem 0;
    transition: all 0.3s ease;
    border-bottom: 1px solid transparent;
    font-weight: 500;
    letter-spacing: 0.5px;
}

.nav-sidebar a:hover {
    color: #ecf0f1;
    padding-left: 0.5rem;
    padding-right: 0.5rem;
    background-color: #1d1d1d;
}

.nav-close-btn {
    align-self: flex-end;
    background: none;
    border: none;
    font-size: 2rem;
    color: white;
    margin-bottom: 1rem;
    cursor: pointer;
    width: 40px;
    height: 40px;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: background-color 0.3s ease;
}

.nav-close-btn:hover {
    background-color: #1d1d1d;
}

/* Overlay */
.nav-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
    z-index: 1050;
}

/* Responsive Styles */

/* Desktop View (>1024px) - Show 5 items */
@media (min-width: 1025px) {
    .desktop {
        display: flex;
    }

    .nav-item {
        padding: 20px 18px;
    }
}

/* Tablet View (769px - 1024px) - Show 4 items */
@media (min-width: 769px) and (max-width: 1024px) {
    .desktop {
        display: flex;
    }

    .nav-item {
        padding: 20px 12px;
    }

    .nav-nav-links a {
        font-size: 13px;
    }
}

/* Mobile View (≤768px) - Show no items in navbar */
@media (max-width: 768px) {
    .desktop {
        display: none;
    }

    .nav-sidebar {
        width: 280px;
        right: -280px;
    }

    /* RTL mode mobile */
    .nav-sidebar.rtl-mode {
        left: -280px;
    }

    .nav-logo {
        padding: 15px 5px;
    }

    .nav-hamburger {
        margin-right: 15px;
        margin-left: 15px;
    }
.nav-navbar{
padding: 0px 10px;
}
}

/* Small Mobile View (≤480px) */
@media (max-width: 480px) {
    .nav-sidebar {
        width: 100%;
        right: -100%;
    }

    /* RTL mode small mobile */
    .nav-sidebar.rtl-mode {
        left: -100%;
    }

    .nav-logo {
        padding: 10px 5px;
    }

    .nav-logo img {
        height: 32px;
    }
}

.rtl-mode {
    border: 0px !important;
    border-radius: 0 !important;
}
</style>
