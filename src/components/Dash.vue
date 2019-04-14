<template>
	<div class="container is-fullheight">
			
		<div v-for="repo in repos" :key="repo.id">
			<Item  :data="repo" />
		</div>

		<a v-if="bottomOfWindow" class="button is-loading">Loading</a>

	</div>
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
			.catch(err => {
				// eslint-disable-next-line
				console.log(err)
			})
		},
		// parsing the list of repos
		parsData (data) {
			// filtring the repos get only thes created in the passed 30 days
			// and map the result to get the neded data
			let mapedData = data.items.filter(repo => {
				if( this.getDaysNb(repo.pushed_at) <= 30) {
					return repo
				}
			}).map(repo => {
					return {
					id: repo.id,
					name: repo.name,
					avatar: repo.owner.avatar_url,
					description: repo.description,
					nbStars: repo.stargazers_count,
					nbEssues: repo.open_issues_count,
					info: `Submitted ${this.getDaysNb(repo.pushed_at)} days ago by ${repo.owner.login}`
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
			this.bottomOfWindow = false
			
		},
		// get the number of days from the creation of the repo to the current date
		getDaysNb (date) {
			return parseInt((Date.now() - new Date(date).getTime()) / 86400000)
		},
		scroll () {
			// add scroll event listner to fitch new repos by hiting the bottom of the page 
			window.onscroll = () => {
				this.bottomOfWindow = document.documentElement.scrollTop + window.innerHeight == document.documentElement.offsetHeight
				if (this.bottomOfWindow) {
					this.getData(this.page)
				}
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