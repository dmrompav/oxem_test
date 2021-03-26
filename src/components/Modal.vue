<template lang="pug">
	.modal-container(
		v-if="!isDataTypeGotten"
	)
		.bg-disable
			.bg-disable__loading(
				v-if="isDataTypeLoading"
			) Loading...
		.modal(
			v-if="!isDataTypeLoading"
		)
			p.modal__p Выберите тип данных:
			button.modal__btn(
				value="small"
				@click="$emit('data-type-select', $event.target.value)"
			) Малый
			button.modal__btn(
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
</style>
