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
</style>
