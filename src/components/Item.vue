<template>
	<div>
		<div class="left">
			<img class="avatar" :src="data.avatr">
		</div>
		<div class="right">
			<span calss="title">
				{{data.name}}
			</span>
			<p class="discription">
				{{data.description}}
			</p>
			<div class="counts">
				<span class="stars">
					{{data.nbStars}}
				</span>
				<span class="essues">
					{{data.nbEssues}}
				</span>
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