<template>
	<section class="container">
		<Item v-for="repo in repos" :data="repo" />

	</section>
</template>

<script>
import axios from 'axios'
import Item from './Item'

export default {
	name: 'Dash',
	data() {
		return {
			repos: {}
		}
	},
	methods: {
		// getting the data from api
		getData (page = 1) {
			axios.get(`https://api.github.com/search/repositories?q=created:>2017-10-22&sort=stars&order=desc&page=${page}`)
			.then(res => this.parsData(res.data))
			.catch(err => console.log(err))
		},
		// parsing the data 
		parsData (data) {
			let mapedData = data.items.map((repo) => {
				let obj = {
					id: repo.id,
					name: repo.name,
					avatr: repo.owner.avatar_url,
					description: repo.description,
					nbStars: repo.stargazers_count,
					nbEssues: repo.open_issues_count 
				}
				return obj
			})
			// init the repos data set
			this.repos = mapedData
		}
	},
	mounted  () {
		this.getData();
	},
	watch: {
		repos: (val) => {
			console.log(val)
		}
	},
	components: {
		Item
	}

}
</script>

<style>
	.container: {
		
	}
</style>