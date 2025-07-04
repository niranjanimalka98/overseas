<template>
    <div class="rtl-toggle-widget">
        <button @click="togglePanel" class="rtl-arrow-btn" :class="{ active: showPanel }" type="button">
            <span class="arrow-icon">{{ showPanel ? '→' : '←' }}</span>
        </button>
        <div class="rtl-panel" :class="{ open: showPanel }" @click.stop>
            <div class="rtl-content">
                <span class="rtl-label">{{ isRTL ? 'RTL' : 'LTR' }}</span>
                <div class="rtl-switch" @click="toggleRTL">
                    <div class="switch-track" :class="{ active: isRTL }">
                        <div class="switch-thumb"></div>
                    </div>
                </div>
            </div>
        </div>
        <div v-if="showPanel" class="rtl-overlay" @click="closePanel"></div>
    </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const props = defineProps({
    persistState: {
        type: Boolean,
        default: true
    },
    storageKey: {
        type: String,
        default: 'rtl-mode'
    }
})

const emit = defineEmits(['rtl-changed'])

const isRTL = ref(false)
const showPanel = ref(false)
const togglePanel = (e) => {
    e.preventDefault()
    e.stopPropagation()
    showPanel.value = !showPanel.value
}

const closePanel = (e) => {
    if (e) {
        e.preventDefault()
        e.stopPropagation()
    }
    showPanel.value = false
}

const toggleRTL = (e) => {
    e.preventDefault()

    e.stopPropagation()

    isRTL.value = !isRTL.value

    const newDirection = isRTL.value ? 'rtl' : 'ltr'

    document.documentElement.dir = newDirection

    document.documentElement.setAttribute('dir', newDirection)

    if (isRTL.value) {
        document.body.classList.add('rtl-mode')
        document.body.classList.remove('ltr-mode')
    } else {
        document.body.classList.add('ltr-mode')
        document.body.classList.remove('rtl-mode')
    }

    if (props.persistState) {
        localStorage.setItem(props.storageKey, isRTL.value.toString())
    }

    emit('rtl-changed', {
        isRTL: isRTL.value,
        direction: newDirection
    })

    setTimeout(() => {
        closePanel()
    }, 300)
}

const loadSavedState = () => {
    if (props.persistState) {
        const saved = localStorage.getItem(props.storageKey)
        if (saved !== null) {
            isRTL.value = saved === 'true'
        }
    }
}

const initializeDirection = () => {
    loadSavedState()

    const initialDirection = isRTL.value ? 'rtl' : 'ltr'
    document.documentElement.dir = initialDirection
    document.documentElement.setAttribute('dir', initialDirection)

    document.body.classList.add(isRTL.value ? 'rtl-mode' : 'ltr-mode')
}

const handleEscape = (e) => {
    if (e.key === 'Escape' && showPanel.value) {
        closePanel()
    }
}

onMounted(() => {
    initializeDirection()
    document.addEventListener('keydown', handleEscape)
})

onUnmounted(() => {
    document.removeEventListener('keydown', handleEscape)
})
</script>

<style scoped>
.rtl-toggle-widget {
    position: fixed;
    top: 50%;
    right: 0;
    transform: translateY(-50%);
    z-index: 1000;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.rtl-arrow-btn {
    position: relative;
    width: 32px;
    height: 48px;
    background: #2196F3;
    border: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: -2px 0 8px rgba(0, 0, 0, 0.15);
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    z-index: 1001;
    -webkit-tap-highlight-color: transparent;
    touch-action: manipulation;
    pointer-events: auto;
}

.rtl-arrow-btn:hover {
    background: #1976D2;
    transform: translateX(-2px);
}

.rtl-arrow-btn:active {
    background: #1565C0;
}

.rtl-arrow-btn.active {
    background: #1565C0;
    border-radius: 0;
}

.arrow-icon {
    color: white;
    font-size: 14px;
    font-weight: bold;
    transition: transform 0.3s ease;
    pointer-events: none;
}

.rtl-arrow-btn.active .arrow-icon {
    transform: rotate(360deg);
}

.rtl-panel {
    position: absolute;
    top: 0;
    right: 32px;
    width: 100px;
    height: 48px;
    background: white;
    border: 1px solid #e0e0e0;
    border-right: none;
    box-shadow: -4px 0 12px rgba(0, 0, 0, 0.15);
    transform: translateX(100%);
    opacity: 0;
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    overflow: hidden;
    -webkit-tap-highlight-color: transparent;
    pointer-events: auto;
}

.rtl-panel.open {
    transform: translateX(0);
    opacity: 1;
    pointer-events: auto;
}

.rtl-content {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 12px;
    height: 100%;
    gap: 8px;
    pointer-events: auto;
}

.rtl-label {
    font-size: 12px;
    font-weight: 600;
    color: #333;
    min-width: 24px;
    pointer-events: none;
}

.rtl-switch {
    cursor: pointer;
    display: flex;
    align-items: center;
    -webkit-tap-highlight-color: transparent;
    touch-action: manipulation;
    pointer-events: auto;
    padding: 4px;
    margin: -4px;
}

.switch-track {
    position: relative;
    width: 36px;
    height: 20px;
    background: #e0e0e0;
    border-radius: 10px;
    transition: all 0.3s ease;
    pointer-events: none;
}

.switch-track.active {
    background: #2196F3;
}

.switch-thumb {
    position: absolute;
    top: 2px;
    left: 2px;
    width: 16px;
    height: 16px;
    background: white;
    border-radius: 50%;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
    pointer-events: none;
}

.switch-track.active .switch-thumb {
    transform: translateX(16px);
}

.rtl-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: transparent;
    z-index: 999;
    -webkit-tap-highlight-color: transparent;
    pointer-events: auto;
}

:global(.rtl-mode) .rtl-toggle-widget {
    right: auto;
    left: 0;
}

:global(.rtl-mode) .rtl-arrow-btn {
    border-radius: 0 8px 8px 0;
}

:global(.rtl-mode) .rtl-panel {
    right: auto;
    left: 32px;
    border-radius: 0 8px 8px 0;
    border-left: none;
    border-right: 1px solid #e0e0e0;
    box-shadow: 4px 0 12px rgba(0, 0, 0, 0.15);
}

:global(.rtl-mode) .rtl-content {
    flex-direction: row-reverse;
}

@media (hover: hover) {
    .rtl-switch:hover .switch-track {
        background: #bdbdbd;
    }

    .rtl-switch:hover .switch-track.active {
        background: #1976D2;
    }

    .rtl-switch:hover .switch-thumb {
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
    }
}

.rtl-panel * {
    transition: all 0.2s ease;
}

@media (max-width: 768px) {
    .rtl-toggle-widget {
        top: 50%;
        transform: translateY(-50%);
    }

    .rtl-arrow-btn {
        width: 40px;
        height: 50px;
        font-size: 16px;
    }

    .rtl-panel {
        right: 40px;
        width: 110px;
        height: 50px;
    }

    .rtl-content {
        padding: 0 12px;
        gap: 8px;
    }

    .rtl-label {
        font-size: 12px;
        min-width: 24px;
    }

    .switch-track {
        width: 36px;
        height: 20px;
    }

    .switch-thumb {
        width: 16px;
        height: 16px;
    }

    .switch-track.active .switch-thumb {
        transform: translateX(16px);
    }

    .rtl-switch {
        padding: 8px;
        margin: -8px;
    }

    :global(.rtl-mode) .rtl-panel {
        left: 40px;
    }
}

@media (max-width: 480px) {
    .rtl-toggle-widget {
        top: 50%;
        transform: translateY(-50%);
    }

    .rtl-arrow-btn {
        width: 36px;
        height: 46px;
    }

    .rtl-panel {
        right: 36px;
        width: 100px;
        height: 46px;
    }

    :global(.rtl-mode) .rtl-panel {
        left: 36px;
    }
}


@media (max-width: 768px) and (orientation: landscape) {
    .rtl-toggle-widget {
        top: 50%;
        transform: translateY(-50%);
    }
}

@media (max-width: 768px) {

    .rtl-arrow-btn,
    .rtl-switch,
    .rtl-overlay {
        -webkit-touch-callout: none;
        -webkit-user-select: none;
        -khtml-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
    }
}
</style>