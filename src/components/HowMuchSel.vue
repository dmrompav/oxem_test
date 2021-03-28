<template lang="pug">
	.paging
		.paging__label Отображать по: 
		button.paging__selector {{ howMuchData }}
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
	position relative
	z-index 20
	margin 10px 0
	display flex
	justify-content flex-start
	align-items center

	&__label
		font-size 20px
		margin 0 10px 0 0

	&__selector
		position relative
		margin 0px 10px
		padding 10px
		width 50px
		background #fff
		color #000
		border-radius 5px
		cursor pointer
		&:hover>ul
			height auto
			max-height 180px
			overflow-y auto

	&__list
		position absolute
		left 0
		top 30px
		height 0
		width 80px
		text-align center
		overflow hidden
		border-radius 5px

	&__option
		background #eee
		padding 10px 5px
		
		&:hover
			background gray

		&--selected
			background #2ecc71
</style>
