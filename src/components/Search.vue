<template lang="pug">
	.search
		input.search__input(
			@input="inputSearch"
			:value="searchText"
			@keyup.enter="search"
			:type="search"
		)
		button.search__submit(
			@click="search"
		) &#128269
		span.search__how-much(
			v-if="showHowMuchResults"
		) Найдено: {{ tableDataLength }}
		.search__results(
			v-if="dirtyInput"
		)
			.search__result(
				v-for="res in results"
				@click="resClick(res)"
			) {{ res }}
</template>


// ===== SCRIPTS ==============================
<script>
export default {
	name: 'Search',
	props: {
		tableDataLength: Number,
		dataCopy: Array,
		tableHead: Array,
	},
	data() {
		return {
			searchText: '',
			showHowMuchResults: false,
			results: [],
			maxResults: 6,
			dirtyInput: false,
		}
	},
	methods: {
		inputSearch(e) {
			this.dirtyInput = true
			this.results = []
			this.searchText = e.target.value
			this.dataCopy.forEach(person => {
				this.tableHead.forEach(key => {
					if(this.results.length < this.maxResults) {
						if(person[key].toString().toLowerCase().includes(this.searchText.toString().toLowerCase())) { 
							this.results.push(person[key])
						}
					}
				})
			})
		},
		search() {
			this.dirtyInput = false
			this.$emit('filterData', this.searchText)
			this.showHowMuchResults = true
		},
		resClick(res) {
			this.searchText = res
			this.search()
			this.dirtyInput = false
		}
	},
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.search
	z-index 5
	position relative
	margin 15px 0
	display flex
	justify-content flex-start
	align-items center

	&__input
		width 150px
		padding 5px
		border-radius 5px 0 0 5px
		&:focus
			box-shadow 0 0 2px 3px #27ae60

	&__submit
		height 26px
		margin-right 10px
		padding 0 10px
		background #2ecc71
		border-radius 0 5px 5px 0

	&__how-much
		font-size 12px

	&__results
		position absolute
		top 30px
		left 0
		width 150px
		overflow hidden
		background #eee
		border-radius 5px

	&__result
		color #000
		cursor pointer
		padding 5px

		&:hover
			background #aaa


</style>
