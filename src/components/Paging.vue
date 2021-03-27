<template lang="pug">
	.paging
		.paging__label Страница: 
		button.paging__btn(
			v-if="whichPage > 1"
			@click="$emit('which-page', whichPage - 1)"
		) &lt; 
		.paging__selector {{ whichPage }}
			ul.paging__list
				li.paging__option(
					v-for="i in howMuchPages"
					:key="i"
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
	methods: {
		chooseRightPage() {
			if(this.whichPage > this.howMuchPages) {
				this.$emit('which-page', this.howMuchPages)
			} else if(this.whichPage < 1) {
				this.$emit('which-page', 1)
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
<style lang="stylus">
.paging
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
		&:hover>ul
			height auto

	&__list
		position absolute
		left 0
		top 30px
		height 0
		width 50px
		text-align center
		overflow hidden
		background #fff

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

</style>
