<template>
    <div class="checkbox-wrapper-33">
        <label class="checkbox" :for="id">
            <input :id="id" type="checkbox" class="checkbox__trigger visuallyhidden" :checked="modelValue"
                @change="$emit('update:modelValue', $event.target.checked)" />
            <span class="checkbox__symbol">
                <svg aria-hidden="true" class="icon-checkbox" width="28" height="28" viewBox="0 0 28 28">
                    <path d="M4 14l8 7L24 7" />
                </svg>
            </span>
            <p class="checkbox__textwrapper">
                <slot>I agree</slot>
            </p>
        </label>
    </div>
</template>

<script setup>
defineProps({
    modelValue: Boolean,
    label: {
        type: String,
        default: 'Checkbox',
    },
})
defineEmits(['update:modelValue'])
</script>

<style scoped>
.checkbox-wrapper-33 {
    --s-xsmall: 0.625em;
    --s-small: 1.2em;
    --border-width: 1px;
    --c-primary: #ffffff;
    --c-primary-20-percent-opacity: rgb(95 17 232 / 20%);
    --c-primary-10-percent-opacity: rgb(95 17 232 / 10%);
    --t-base: 0.4s;
    --t-fast: 0.2s;
    --e-in: ease-in;
    --e-out: cubic-bezier(0.11, 0.29, 0.18, 0.98);
}

.visuallyhidden {
    border: 0;
    clip: rect(0 0 0 0);
    height: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute;
    width: 1px;
}

.checkbox {
    display: flex;
    align-items: center;
    justify-content: flex-start;
}

.checkbox+.checkbox {
    margin-top: var(--s-small);
}

.checkbox__symbol {
    display: flex;
    margin-right: calc(var(--s-small) * 0.7);
    border: var(--border-width) solid var(--c-primary);
    position: relative;
    border-radius: 0.1em;
    width: 1.5em;
    height: 1.5em;
    transition:
        box-shadow var(--t-base) var(--e-out),
        background-color var(--t-base);
    box-shadow: 0 0 0 0 var(--c-primary-10-percent-opacity);
}

.checkbox__symbol:after {
    content: '';
    position: absolute;
    top: 0.5em;
    left: 0.5em;
    width: 0.25em;
    height: 0.25em;
    background-color: var(--c-primary-20-percent-opacity);
    opacity: 0;
    border-radius: 3em;
    transform: scale(1);
    transform-origin: 50% 50%;
}

.icon-checkbox {
    width: 1em;
    height: 1em;
    margin: auto;
    fill: none;
    stroke-width: 3;
    stroke: currentColor;
    stroke-linecap: round;
    stroke-linejoin: round;
    stroke-miterlimit: 10;
    color: var(--c-primary);
    display: inline-block;
}

.icon-checkbox path {
    transition: stroke-dashoffset var(--t-fast) var(--e-in);
    stroke-dasharray: 30px, 31px;
    stroke-dashoffset: 31px;
}

.checkbox__textwrapper {
    margin: 0;
}

.checkbox__trigger:checked+.checkbox__symbol:after {
    animation: ripple-33 1.5s var(--e-out);
}

.checkbox__trigger:checked+.checkbox__symbol .icon-checkbox path {
    transition: stroke-dashoffset var(--t-base) var(--e-out);
    stroke-dashoffset: 0px;
}

.checkbox__trigger:focus+.checkbox__symbol {
    box-shadow: 0 0 0 0.25em var(--c-primary-20-percent-opacity);
}

@keyframes ripple-33 {
    from {
        transform: scale(0);
        opacity: 1;
    }

    to {
        opacity: 0;
        transform: scale(20);
    }
}
</style>
