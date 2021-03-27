<template lang="pug">
	details.add-new
		summary.add-new__summary Добавить
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
				)
				p.error(
					v-if="$v.form.id.$dirty && $v.form[title].$invalid"
				) Обязательное поле
				p.error(
					v-if="title==='id' && $v.form.id.$dirty && $v.form.id.$invalid"
				) ID занят
				p.error(
					v-if="title==='email' && $v.form.email.$dirty && $v.form.email.$invalid"
				) Некорректный email
				p.error(
					v-if="title==='phone' && $v.form.phone.$dirty && $v.form.phone.$invalid"
				) Номер в формате: (123)123-1234
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
			formMask: {
				phone: '(___)___-____'
			}
		}
	},
	methods: {
		checkAndAdd(e) {
			this.$v.form.$touch()
			if(this.$v.form.$invalid) {
				e.preventDefault()
			} else {
				this.$emit('add-new', this.form)
			}
		},
		phoneMask() {
		}
	},
	validations: {
		form: {
			id: {
				required,
				valid(value) {
					return !this.tableIds.includes(value)
				}
			},
			firstName: {
				required,
			},
			lastName: {
				required,
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
</style>
