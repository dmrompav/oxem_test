<template lang="pug">
	.paging
		label.paging__label Отображать по: 
		select.paging__selector
			option.paging__option(
				v-for="i in howMuchSelect"
				:selected="i===howMuchData"
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
</style>
