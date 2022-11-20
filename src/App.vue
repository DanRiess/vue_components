<script setup lang="ts">
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import HamburgerMenu from './components/buttons/HamburgerMenu.vue'
</script>

<template>
	<article class="flow main-content">
		<h1>Vue Components</h1>
		<h2>Buttons</h2>
		<div class="square-buttons">
			<HamburgerMenu v-bind="{ width: 30 }" />
		</div>
	</article>
</template>

<style scoped>
.main-content {
	display: flex;
	place-items: center;
	flex-direction: column;
}

.square-buttons {
	padding: 3rem;
	max-width: 1200px;
	display: flex;
	gap: clamp(2rem, 8vw, 4rem);
}
</style>
