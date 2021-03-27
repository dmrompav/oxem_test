<template lang="pug">
	.info(
		v-if="selectedPerson"
	)
		p.info__p Выбран пользователь 
			b {{ selectedPerson.firstName + ' ' + selectedPerson.lastName }}
		p.info__p Описание: 
		textarea.info__textarea(disabled) {{ selectedPerson.description }}
		p.info__p Адрес проживания: 
			b {{ selectedPerson.address.streetAddress }}
		p.info__p Город: 
			b {{ selectedPerson.address.city }}
		p.info__p Провинция/штат: 
			b {{ selectedPerson.address.state }}
		p.info__p Индекс: 
			b {{ selectedPerson.address.zip }}
</template>


// ===== SCRIPTS ==============================
<script>
export default {
	name: 'Info',
	props: {
		selectedPerson: Object,
	},
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.info
	font-size 18px
	&__p
		margin 10px 0 0 0

	&__textarea
		padding 5px
		margin 0 0 10px 0
</style>
