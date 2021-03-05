<template>
	<div class="card">
		<h2 class="town-name">{{cityName}}</h2>
		<div class="temperature">{{temperature}}°C</div>
		<div class="weather-effects">{{weatherEffects}}</div>
	</div>
</template>

<script>
	export default {
		name: "city-card",
		props: ['cityName', 'temperature', 'weatherEffects']
	}
</script>

<style scoped>
	.card {
		border: var(--primaty-color) solid 2px;
		border-radius: 7px;
		padding: 15px;

		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.town-name {
		margin: 0 0 25px 0px;
	}
	.temperature {
		font-size: 28px;
		margin-bottom: 5px;
	}
	.weather-effects {
		font-size: 18px;
	}
</style>