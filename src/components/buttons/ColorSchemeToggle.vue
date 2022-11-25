<script setup lang="ts">
import {computed, CSSProperties} from 'vue'

// create an interface for the props
export interface Props {
    borderColor?: string
    borderWidth?: string
    dayColor?: string
    nightColor?: string
    width?: number
    onHoverScale?: string
}

// create default values for the interface Props
const props = withDefaults(defineProps<Props>(), {
    borderColor: '#f0f0f0',
    borderWidth: '2px',
    dayColor: '#f0f0f0',
    nightColor: '#f0f0f0',
    width: 30,
    onHoverScale: '1.1'
})

// define the props as computed property to use on the button
const cssProps = computed<CSSProperties>(() => {
    return {
        '--border-color': props.borderColor,
		'--border-width': props.borderWidth,
		'--day-color': props.dayColor,
		'--night-color': `${props.nightColor}`,
		'--width': props.width > 24 ? `${props.width}px` : '25px',
		'--on-hover-scale': props.onHoverScale,
    }
})

const toggleColorScheme = (e: MouseEvent) => {
    const button = e.target as HTMLButtonElement
    const currentState: String | null = button.getAttribute('data-state')

    if (currentState === 'night') {
        button.setAttribute('data-state', 'day')
    } else {
        button.setAttribute('data-state', 'night')
    }
}
</script>

<template>
    <button 
    class="color-scheme-button" 
    aria-controls="color-scheme-toggle" 
    data-state="night" 
    :style="cssProps"
    @click="toggleColorScheme">
        <svg class="color-scheme-svg" stroke="var(--button-color)" fill="none" viewBox="0 0 100 100">
            <path 
                class="path-sun"
                stroke-width="4"
				stroke-linecap="round"
				stroke-linejoin="round" 
                d="m 50 10 a 40 40 0 0 0 -40 40 a 40 40 0 0 0 40 40 a 40 40 0 0 0 40 -40 a 40 40 0 0 0 -40 -40" />
            <path class="path-moon" stroke-width="4" stroke-linecap="round" d="m 50 10 a 45 45 0 0 0 20 75" />
            <path class="path-decoration" stroke-width="2" stroke-linecap="round" d="m 50 20 a 30 30 0 0 0 0 60" />
            <path class="path-star-large path-star-1" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" d="m 70 30 a 10 10 0 0 1 10 -10 v 30 h 15" />
            <path class="path-star-large path-star-2" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" d="m 60 20 a 10 10 0 0 1 10 10 l -20 50 v 15" />
            <path class="path-star-large path-star-3" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" d="m 70 10 a 10 10 0 0 1 -10 10 l -40 30 h -15" />
            <path class="path-star-large path-star-4" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" d="m 80 20 a 10 10 0 0 1 -10 -10 l -20 10 v -15" />
            <path class="path-star-small path-star-5" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" d="m 64 50 a 7 7 0 0 0 -7 7 l 38 38" />
            <path class="path-star-small path-star-6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" d="m 57 57 a 7 7 0 0 0 -7 -7 l -45 45" />
            <path class="path-star-small path-star-7" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" d="m 50 50 a 7 7 0 0 0 7 -7 l -32 -18 l -20 -20" />
            <path class="path-star-small path-star-8" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" d="m 57 43 a 7 7 0 0 0 7 7 l 11 -25 l 20 -20" />
        </svg>
    </button>
</template>

<style scoped>
button {
	background: transparent;
	border-width: var(--border-width);
	border-style: solid;
	border-color: var(--border-color);
	border-radius: 20%;
	cursor: pointer;
    width: calc(var(--border-width) * 2 + var(--width));
    height: calc(var(--border-width) * 2 + var(--width));
}

.color-scheme-button {
    transition: scale 0.2s ease;
    will-change: scale;
}

.color-scheme-button:hover {
	scale: var(--on-hover-scale);
}

.color-scheme-button * {
    pointer-events: none;
}

.color-scheme-svg path {
    transition: 500ms;
    transform-origin: center;
}

.color-scheme-button[data-state='night'] {
    stroke: var(--night-color);
}

.color-scheme-svg .path-sun {
    stroke-dasharray: calc(1.16 * 3.14 * 40) 1000;
}

.color-scheme-svg .path-decoration {
    stroke-dasharray: 5 10 30 200;
    stroke-dashoffset: -40;
}

.color-scheme-svg .path-moon {
    stroke-dasharray: calc(0.66 * 3.14 * 45) 100;
}

.color-scheme-svg .path-star-large {
    stroke-dasharray: calc(0.5 * 3.14 * 10) 1000;
}

.color-scheme-svg .path-star-small {
    stroke-dasharray: calc(0.5 * 3.14 * 7) 1000;
}

/* EFFECTS */
.color-scheme-button[data-state='day'] {
    stroke: var(--day-color);
    background: white;
}

.color-scheme-button[data-state='day'] svg {
    stroke: #202020;
}
.color-scheme-button[data-state='day'] .path-sun {
    scale: 0.8;
    stroke-dasharray: calc(2 * 3.14 * 40);
}

.color-scheme-button[data-state='day'] .path-moon {
    scale: 0.8;
    stroke-dashoffset: -100;
}

.color-scheme-button[data-state='day'] .path-decoration {
    scale: 0.7;
    stroke-width: 4;
    stroke-dashoffset: 0;
}

.color-scheme-button[data-state='day'] .path-star-1 {
    stroke-dashoffset: -54;
}

.color-scheme-button[data-state='day'] .path-star-2 {
    stroke-dashoffset: -79;
}

.color-scheme-button[data-state='day'] .path-star-3 {
    stroke-dashoffset: -74;
}

.color-scheme-button[data-state='day'] .path-star-4 {
    stroke-dashoffset: -47;
}

.color-scheme-button[data-state='day'] .path-star-5 {
    stroke-width: 3;
    stroke-dashoffset: -39;
}
.color-scheme-button[data-state='day'] .path-star-6 {
    stroke-width: 3;
    stroke-dashoffset: -49;
}
.color-scheme-button[data-state='day'] .path-star-7 {
    stroke-width: 3;
    stroke-dashoffset: -50;
}
.color-scheme-button[data-state='day'] .path-star-8 {
    stroke-width: 3;
    stroke-dashoffset: -41;
}
</style>