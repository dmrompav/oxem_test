<template lang="pug">
	.search
		input.search__input(
			v-model="searchText"
			@keyup.enter="search"
			:type="search"
		)
		button.search__submit(
			@click="search"
		) &#128269
		span.search__how-much(
			v-if="showHowMuchResults"
		) Найдено: {{ tableDataLength }}
</template>


// ===== SCRIPTS ==============================
<script>
export default {
	name: 'Search',
	props: {
		tableDataLength: Number,
	},
	data() {
		return {
			searchText: '',
			showHowMuchResults: false,
		}
	},
	methods: {
		search() {
			this.$emit('filterData', this.searchText)
			this.showHowMuchResults = true
		},
	}
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.search
	margin 15px 0
	display flex
	justify-content flex-start
	align-items center

	&__input
		width 150px
		padding 5px
		border-radius 5px 0 0 5px

	&__submit
		height 26px
		margin-right 10px
		padding 0 10px
		background #2ecc71
		border-radius 0 5px 5px 0

	&__how-much
		font-size 12px


</style>
