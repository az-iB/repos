<template>
	<section class="container">
		<div v-for="repo in repos" :key="repo.id">
			<Item  :data="repo" />
		</div>

	</section>
</template>

<script>
import axios from 'axios'
import Item from './Item'

export default {
	name: 'Dash',
	data() {
		return {
			repos: [],
			items: [],
			bottomOfWindow: false,
			page: 0
		}
	},
	methods: {
		// getting the data from api
		getData (page = 0) {
			// eslint-disable-next-line
			console.log(page)
			axios.get(`https://api.github.com/search/repositories?q=created:>2017-10-22&sort=stars&order=desc&page=${page}`)
			.then(res => this.parsData(res.data))
		},
		// parsing the data 
		parsData (data) {
			let mapedData = data.items.map((repo) => {
				return {
					id: repo.id,
					name: repo.name,
					avatar: repo.owner.avatar_url,
					description: repo.description,
					nbStars: repo.stargazers_count,
					nbEssues: repo.open_issues_count 
				}
			})
			this.page += 1

			if (this.repos.length > 0) {
				this.repos = this.repos.filter( repo => ! mapedData.find ( maped => repo['id'] === maped['id']) ).concat(mapedData)
			} else {
				this.repos = mapedData
			}
			
		},
		scroll () {
			window.onscroll = () => {
				this.bottomOfWindow = document.documentElement.scrollTop + window.innerHeight === document.documentElement.offsetHeight
			}
		},
	},
	beforeMount () {
		this.getData()
	},
	created () {
		this.scroll()
	},
	watch: {
		repos () {
			// eslint-disable-next-line
			console.log(this.repos.length)
		},
		bottomOfWindow (val) {
			// eslint-disable-next-line
			console.log(val)
			if (val) {
				this.getData(this.page)
			}
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