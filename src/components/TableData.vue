<template lang="pug">
	table.table
		caption.table__caption
			h1 Test Дмитрий Павлов
		tr.table__header
			td.table__cell №
			th.table__cell.table__cell_header(
				v-for="(colName, key) in tableHead"
				@click="$emit('sort-data-click', colName, key)"
			) 
				span.sort(v-if="whichSort[key]" v-html="whichSort[key]==='ascend' ? '&uarr;' : '&darr;'")
				| {{ colName }}
		tr.table__row(
			v-for="(person, key) in cutted"
			@click="rowSelect(person, key)"
			:class="whichRow === key ? 'table__row--selected' : ''"
		)
			td.table__cell {{ (whichPage - 1) * howMuchData + key + 1 }}
			td.table__cell(
				v-for="colName in tableHead"
			) {{ person[colName] }}
</template>


// ===== SCRIPTS ==============================
<script>
export default {
	name: 'TableData',
	props: {
		tableHead: Array,
		whichPage: Number,
		whichSort: Array,
		cutted: Array,
		howMuchData: Number,
	},
	data() {
		return {
			whichRow: -1,
		}
	},
	methods: {
		rowSelect(person, key) {
			this.$emit('selected-person', person);
			this.whichRow = key
		}
	}
}
</script>


// ===== STYLES ==============================
<style scoped lang="stylus">
.table
	margin 20px 0 30px 0
	border-collapse: collapse

	&__caption
		font-size 30px
		margin 15px 0

	&__header
		background #2c3e50
		font-size 20px
		cursor pointer

	&__row
		cursor pointer
		&:hover
			background gray

		&--selected
			background #8e44ad

	&__cell
		border 1px solid
		padding 3px 5px

		&_header
			border 2px solid
			transition background 0.1s
			&:hover
				background #27ae60

.sort
	margin-right 3px

</style>
