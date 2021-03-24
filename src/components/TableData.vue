<template lang="pug">
	//- dev ============
	mixin Paging
		.paging
			label.paging__label Страница: 
			button.paging__btn &lt; 
			select.paging__selector
				option(
					v-for=""
				).paging__option
			button.paging__btn &gt;

	- var howmuch = "10"
	mixin HowMuchData (howmuch)
		.paging
			label.paging__label Отображать по: 
			select.paging__selector
				
				option.paging__option= howmuch

	//- prod ===========
	.table-data
		+HowMuchData
		+Paging
		table.table
			caption.table__caption
			tr.table__row.table__row_header
				th.table__cell.table__cell_header(
					v-for=""
				)
			tr.table__row
				td.table__cell
		+Paging

</template>

<script>
export default {
	name: 'TableData',
	data() {
		return {
			tableData: [
				{
					id: 101,
					firstName: 'Sue',
					lastName: 'Corson',
					email: 'DWhalley@in.gov',
					phone: '(612)211-6296',
					address: {
						streetAddress: '9792 Mattis Ct',
						city: 'Waukesha',
						state: 'WI',
						zip: '22178'
					},
					description: 'et lacus magna dolor...',
				},
			]
		}
	},
	computed: {

	},
	methods: {

	},
	created: function() {

	}
}
</script>

<style scoped lang="stylus">

</style>
