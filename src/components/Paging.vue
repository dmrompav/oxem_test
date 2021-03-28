<template lang="pug">
	.paging
		.paging__label Страница: 
		button.paging__btn(
			v-if="whichPage > 1"
			@click="$emit('which-page', whichPage - 1)"
		) &lt; 
		button.paging__selector {{ whichPage }}
			ul.paging__list
				li.paging__option(
					v-for="i in howMuchPages"
					:class="i===whichPage ? 'paging__option--selected' : ''"
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
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.paging
	position relative
	z-index 10
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
		cursor pointer
		border-radius 5px
		&:hover>ul
			height auto
			max-height 180px
			overflow-y auto

	&__list
		display block
		position absolute
		z-index 1
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

	&__btn
		padding 10px
		background #2ecc71
		transition background 0.1s
		border-radius 5px
		&:hover
			background #2ecc71

</style>
