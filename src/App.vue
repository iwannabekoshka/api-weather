<template>
	<div id="app">
		<form class="form-add" @submit.prevent="addTown">
			<input type="text" v-model="newTown" placeholder="Enter city name...">
			<button type="submit">Add</button>
			<div style="position: absolute; left: 255px; top: 0;">
				<loading v-if="loading"/>
			</div>
			<div class="error" v-if="error">{{error}}</div>
		</form>

		<div class="cities">
			<city-card
					v-for="city in cities" :key="city.id"
					:city-name="city.cityName"
					:temperature="city.temperature"
					:weather-effects="city.weatherEffects"
			/>
		</div>

	</div>
</template>

<script>
	import CityCard from "@/components/city-card";
	import Loading from "@/components/loading";
	export default {
		name: 'App',
		components: {Loading, CityCard},
		data() {
			return {
				error: '',
				loading: false,
				newTown: '',
				cities: []
			}
		},
		methods: {
			addTown() {
				this.error = ''
				this.loading = true
				const that = this

				fetch(`http://api.openweathermap.org/data/2.5/weather
				?q=${this.newTown}
				&appid=7b02d2f234d32440ccfe42976eb070d4`)
					.then(response => {
						return response.json()
					})
					.then(function(data) {
						// если есть сообщение об ошибке, значит, ошибка
						if (data.message) {
							that.error = data.message
							that.loading = false
						// если такого города еще нет
						} else if (that.cities.findIndex(city => city.id === data.id) === -1) {
							that.cities.push(
								{
									id: data.id,
									cityName: data.name,
									temperature: Math.round(data.main.temp-273),
									weatherEffects: data.weather[0].description
								}
							)

							that.loading = false
							that.newTown = ''
						// если такой город есть
						} else {
							that.error = 'City already chosen'
							that.loading = false
						}
					})
			}
		},
		mounted() {
		}
	}
</script>

<style>
	body {
		margin: 0;
		padding: 0;

		box-sizing: border-box;

		--primaty-color: #32cd32;
		--border: 2px solid limegreen;
	}
	#app {
		font-family: Arial, sans-serif;
		font-size: 24px;

		display: flex;
		flex-direction: column;
		align-items: center;

		width: 100vw;
		padding-top: 100px;
	}
	.cities {
		display: flex;
		/*justify-content: space-between;*/
		align-items: center;
		flex-wrap: wrap;
		gap: 10px;

		width: 80%;

		margin-bottom: 20px;
	}
	.form-add {
		margin-bottom: 20px;
		position: relative;
	}
	input[type="text"] {
		padding: 5px 10px;
		margin-right: 10px;
		border: var(--border);
	}
	input[type="text"]:focus {
		outline: var(--border)
	}
	button {
		border: var(--border);
		background-color: var(--primaty-color);
		color: white;
		font-weight: bold;
		border-radius: 5px;
		padding: 5px 10px;
		cursor: pointer;
	}
	button:hover {
		background-color: #35bb35;
	}
</style>
