<template lang="pug">
	details.add-new
		summary.add-new__summary Добавить
		form.add-new__form
			.add-new__row(
				v-for="(title, i) in tableHead"
			)
				input.add-new__input(
					v-model="form[title]"
					:type="typesHead[i]"
					:placeholder="title"
				)
				p.error(
					v-if="$v.form.phone.$dirty && !$v.form.phone.$valid"
				) Обязательное поле
		button(
			type="submit"
			@click="checkAndAdd"
		).add-new__button Ок
</template>


// ===== SCRIPTS ==============================
<script>
import { validationMixin } from 'vuelidate'
import { required, minLength } from 'vuelidate/lib/validators'

export default {
	name: 'AddNewForm',
	mixins: [validationMixin],
	props: {
		tableHead: Array,
		typesHead: Array,
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
			}
		}
	},
	validations: {
		form: {
			id: {
				required,
			},
			firstName: {
				required,
				minLength: minLength(4)
			},
			lastName: {
				required,
				minLength: minLength(4)
			},
			email: {
				required,
				minLength: minLength(4)
			},
			phone: {
				required,
				valid(value) {
					console.log(value)
					return (/^\([0-9]*3\)[0-9]*3-[0-9]*4/).test(value)
				},
			},
		}
	},

	methods: {
		checkAndAdd() {
			this.$v.form.$touch()
			console.log(this.$v.form.$invalid)
		},
	}
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
</style>
