<template lang="pug">
	details.add-new
		summary.add-new__summary Добавить строку
		form.add-new__form(
			@submit="checkAndAdd"
		)
			.add-new__row(
				v-for="(title, i) in tableHead"
			)
				input.add-new__input(
					v-model="form[title]"
					:type="typesHead[i]"
					:placeholder="title"
					:class="($v.form.id.$dirty && $v.form[title].$invalid)? '.add-new__input--wrong' : ''"
				)
				

				// ===================================
				// * VALIDATION ERRORS ----------------
				p.error(
					v-if="$v.form.$dirty && !$v.form[title].required"
				) Обязательное поле
				p.error(
					v-if="title==='id' && $v.form.$dirty && !$v.form[title].valid"
				) ID занят
				p.error(
					v-if="title==='id' && $v.form.$dirty && !$v.form[title].validNumberOnly"
				) Только цифры
				p.error(
					v-if="title==='firstName' && $v.form.$dirty && !$v.form[title].valid"
				) С заглавной буквы
				p.error(
					v-if="title==='lastName' && $v.form.$dirty && !$v.form[title].valid"
				) С заглавной буквы
				p.error(
					v-if="title==='email' && $v.form.$dirty && !$v.form[title].valid"
				) Некорректный email
				p.error(
					v-if="title==='phone' && $v.form.$dirty && !$v.form[title].valid"
				) Номер в формате: (123)123-1234
				// * VALIDATION ERRORS ----------------
				// ===================================


		button(
			type="submit"
			@click="checkAndAdd"
		).add-new__button Ок
</template>


// ===== SCRIPTS ==============================
<script>
import { validationMixin } from 'vuelidate'
import { required } from 'vuelidate/lib/validators'

export default {
	name: 'AddNewForm',
	mixins: [validationMixin],
	props: {
		tableHead: Array,
		typesHead: Array,
		tableIds: Array,
	},
	data() {
		return {
			ableAdding: false,
			form: {
				id: '',
				firstName: '',
				lastName: '',
				email: '',
				phone: '',
			},
		}
	},
	methods: {
		checkAndAdd(e) {
			this.$v.form.$touch()
			if(this.$v.form.$invalid) {
				e.preventDefault()
			} else {
				this.$emit('add-new', this.form)
				this.form = {
					id: '',
					firstName: '',
					lastName: '',
					email: '',
					phone: '',
				}
				this.$v.$reset()
			}
		},
	},
	validations: {
		form: {
			id: {
				required,
				validNumberOnly(value) {
					return (/^\d*$/).test(value)
				},
				valid(value) {
					return !this.tableIds.includes(value)
				}
			},
			firstName: {
				required,
				valid(value) {
					return (/^[A-Z]/).test(value)
				}
			},
			lastName: {
				required,
				valid(value) {
					return (/^[A-Z]/).test(value)
				}
			},
			email: {
				required,
				valid(value) {
					return value.includes('@')
				},
			},
			phone: {
				required,
				valid(value) {
					return (/^\(\d{3}\)\d{3}-\d{4}/).test(value)
				},
			},
		}
	},
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.add-new
	margin 10px 0

	&__summary
		position relative
		width 200px
		padding 10px 0

		&:before
			content ''
			position absolute
			z-index -1
			top 0
			left 0
			width 0
			transition width 0.1s
			height 100%
			background #e74c3c
			border-radius 5px

		&:hover:before
			width 100%

	&__input
		width 150px
		padding 5px
		border-radius 5px
		margin 5px 0

	&__button
		height 26px
		padding 0 10px
		background #2ecc71
		border-radius 5px
		font-size 20px
		margin 5px 0

.error
	color #e74c3c
	margin 0 0 5px 0

</style>
