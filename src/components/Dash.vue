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
		// getting list of repos from api
		getData (page = 0) {
			axios.get(`https://api.github.com/search/repositories?q=created:>2017-10-22&sort=stars&order=desc&page=${page}`)
			.then(res => this.parsData(res.data))
		},
		// parsing the list of repos
		parsData (data) {
			let mapedData = data.items.map((repo) => {
				return {
					id: repo.id,
					name: repo.name,
					avatar: repo.owner.avatar_url,
					description: repo.description,
					nbStars: repo.stargazers_count,
					nbEssues: repo.open_issues_count,
					info: `Submitted ${this.getDaysNb(repo.pushed_at)} by ${repo.owner.login}`
				}
			})

			this.page += 1

			// afecting parsed data to the repos variable
			if (this.repos.length > 0) {
				// filtring the new data by id
				this.repos = this.repos.filter( repo => ! mapedData.find ( maped => repo['id'] === maped['id']) ).concat(mapedData)
			} else {
				this.repos = mapedData
			}
			
		},
		getDaysNb (date) {
			let nbDays = (Date.now() - new Date(date).getTime()) / 86400000
			if (parseInt(nbDays) >= 1) {
				return `${parseInt(nbDays)} days ago`
			} else {
				return 'less than a day'
			}
		},
		scroll () {
			// add scroll event listner to fitch new repos by hiting the bottom of the page 
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
		bottomOfWindow (val) {
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