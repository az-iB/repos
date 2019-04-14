<template>
	<div class="content is-fluid">
		<div class="columns is-mobile">
			<div class="column">
				<img class="avatar" :src="data.avatar">
			</div>
			<div class="column is-four-fifths">
				<h2 calss="title">
					{{data.name}}
				</h2>
				<p class="discription">
					{{data.description}}
				</p>
				<div class="columns">
					<span class="column stars box">
						Starts : {{data.nbStars}}
					</span>
					<span class=" column essues box">
						Issues : {{data.nbEssues}}
					</span>
					<span class="column is-three-fifths">
						{{data.info}}
					</span>
				</div>
			</div>
		</div>
	</div>
</template>
<script >
	export default {
		props:['data'],
		data () {
			return {

			}
		}
	}
</script>
<style scoped>
	
</style>