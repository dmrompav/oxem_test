<template lang="pug">
	table.table
		caption.table__caption
		tr.table__row.table__row_header
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
.table__row--selected
	background #000
</style>
