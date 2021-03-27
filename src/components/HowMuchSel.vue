<template lang="pug">
	.paging
		.paging__label Отображать по: 
		.paging__selector {{ howMuchData }}
			ul.paging__list
				li.paging__option(
					v-for="i in howMuchSelect"
					:class="i===howMuchData ? 'paging__option--selected' : ''"
					@click="$emit('how-much-data', i)"
				) {{ i }}
</template>


// ===== SCRIPTS ==============================
<script>
export default {
	name: 'HowMuchSel',
	props: {
		howMuchSelect: Array,
		howMuchData: Number,
	},
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.paging
	z-index 10

// using Paging.vue styles
</style>
