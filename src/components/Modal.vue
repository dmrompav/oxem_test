<template lang="pug">
	transition(name="appear")
		.modal-container(
			v-if="!isDataTypeGotten"
		)
			.loading(
				v-if="isDataTypeLoading"
			) 
			.modal(
				v-if="!isDataTypeLoading"
			)
				p.modal__p Выберите тип данных:
				.modal__btns
					button.modal__btn.modal__btn_1(
						value="small"
						@click="$emit('data-type-select', $event.target.value)"
					) Малый
					button.modal__btn.modal__btn_2(
						value="big"
						@click="$emit('data-type-select', $event.target.value)"
					) Большой
</template>


// ===== SCRIPTS ==============================
<script>
export default {
	name: 'Modal',
	props: {
		isDataTypeLoading: Boolean,
		isDataTypeGotten: Boolean,
	},
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.modal-container
	z-index 100
	position absolute
	width 100vw
	height 100vh
	background #2980b9
	display flex
	justify-content center
	align-items center
	opacity 1

.loading
	position absolute
	width 100px
	height 100px
	border 15px solid #fff
	border-top 15px solid rgba(#fff, 0.5)
	border-radius 50%
	animation loading 1s linear 0s infinite

@keyframes loading 
	0%
		transform rotate(0deg)
	100%
		transform rotate(360deg)

.modal
	position absolute
	
	&__p
		font-size 20px
		text-align center

	&__btns
		display flex
		justify-content center
		align-items center
		margin-top 30px

	&__btn
		background #000
		font-size 20px
		margin 10px
		padding 10px
		border-radius 10px
		transition transform 0.1s
		&:hover
			transform scale(1.2)

		&_1
			background #2ecc71
		&_2
			background #e74c3c

.appear-leave
	opacity 1
.appear-leave-active
	transition opacity 0.5s ease
.appear-leave-to
	opacity 0


</style>
