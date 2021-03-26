<template lang="pug">
	.paging
		label.paging__label Страница: 
		button.paging__btn(
			v-if="whichPage > 1"
			@click="$emit('which-page', whichPage - 1)"
		) &lt; 
		select.paging__selector
			option.paging__option(
				v-for="i in howMuchPages"
				:key="i"
				:selected="i === whichPage"
				@click="$emit('which-page', i)"
			) {{ i }}
		button.paging__btn(
			v-if="whichPage < howMuchPages"
			@click="$emit('which-page', whichPage + 1)"
		) &gt;
</template>



// ===== SCRIPTS ==============================
<script>
export default {
	name: 'Paging',
	props: {
		howMuchPages: Number,
		whichPage: Number,
	},
	methods: {
		chooseRightPage() {
			if(this.whichPage > this.howMuchPages) {
				this.$emit('which-page', this.howMuchPages)
			}
		}
	},
	watch: {
		whichPage() { this.chooseRightPage() },
		howMuchPages() { this.chooseRightPage() },
	}
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
</style>
