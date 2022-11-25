<script setup lang="ts">
import { computed, CSSProperties } from 'vue'

// create an interface for the props
export interface Props {
	borderColor?: string
	borderWidth?: string
	hamburgerColor?: string
	closeColor?: string
	width?: number
	onHoverScale?: string
}

// create default values for the interface Props
const props = withDefaults(defineProps<Props>(), {
	borderColor: '#f0f0f0',
	borderWidth: '2px',
	hamburgerColor: '#efd307',
	closeColor: '#ed4337',
	width: 30,
	onHoverScale: '1.1',
})

// define the props as computed property to use on the button
const cssProps = computed<CSSProperties>(() => {
	return {
		'--border-color': props.borderColor,
		'--border-width': props.borderWidth,
		'--hamburger-color': props.hamburgerColor,
		'--close-color': `${props.closeColor}`,
		'--width': props.width > 24 ? `${props.width}px` : '25px',
		'--on-hover-scale': props.onHoverScale,
	}
})

const toggleNavigation = (e: MouseEvent) => {
	const button = e.target as HTMLButtonElement
	const currentState: String | null = button.getAttribute('data-state')

	if (!currentState || currentState === 'closed') {
		button.setAttribute('data-state', 'opened')
		button.setAttribute('aria-expanded', 'true')
	} else if (currentState === 'opened') {
		button.setAttribute('data-state', 'closed')
		button.setAttribute('aria-expanded', 'false')
	}
}
</script>

<template>
	<button
		class="menu-button"
		aria-controls="primary-navigation"
		aria-expanded="false"
		:style="cssProps"
		@click="toggleNavigation">
		<svg class="hamburger" fill="none" viewBox="0 0 100 100">
			<path
				class="path"
				stroke-width="10"
				stroke-linecap="round"
				stroke-linejoin="round"
				d="m 20 30 h 60 a 10 10 0 0 1 0 20 h -60 a 10 10 0 0 0 0 20 h 60 a 35 35 0 0 0 0 -60 h -30 v 90"></path>
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
	width: calc(var(--border-width) * 2 + var(--width));
	height: calc(var(--border-width) * 2 + var(--width));
	cursor: pointer;
}

.menu-button {
	transition: scale 0.2s ease;
	will-change: scale;
}

.menu-button:hover {
	scale: var(--on-hover-scale);
}

.menu-button .path {
	transform-origin: center;
	stroke-dasharray: 60 31 60 31 60 1000;
	stroke: var(--hamburger-color);
}

.menu-button[data-state='closed'] .path {
	animation: to-hamburger 400ms forwards;
}

.menu-button[data-state='opened'] .path {
	animation: to-close 400ms forwards;
	animation-timing-function: ease-in;
}

.menu-button * {
	pointer-events: none;
}

@keyframes to-close {
	0% {
		stroke-dasharray: 60 31 60 31 60 1000;
		stroke-dashoffset: 0;
		stroke: var(--hamburger-color);
	}
	60% {
		stroke-dasharray: 60 31 60 31 60 1000;
		stroke: var(--close-color);
	}
	100% {
		stroke-dasharray: 60 130 1000;
		stroke-dashoffset: -182;
		rotate: 0.125turn;
		translate: 15px -15px;
		stroke: var(--close-color);
	}
}

@keyframes to-hamburger {
	100% {
		stroke-dasharray: 60 31 60 31 60 1000;
		stroke-dashoffset: 0;
		stroke: var(--hamburger-color);
	}
	40% {
		stroke-dasharray: 60 31 60 31 60 1000;
		stroke: var(--close-color);
	}
	0% {
		stroke-dasharray: 60 130 1000;
		stroke-dashoffset: -182;
		rotate: 0.125turn;
		translate: 15px -15px;
	}
}
</style>
