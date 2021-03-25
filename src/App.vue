<template lang="pug">
	#app
		//* ========== DEV ============
		mixin HowMuchData
			.paging
				label.paging__label Отображать по: 
				select.paging__selector
					option.paging__option(
						v-for="i in howMuchSelect"
						:selected="i===howMuchData"
						@click="() => howMuchData = i"
					) {{ i }}
		mixin Paging
			.paging
				label.paging__label Страница: 
				button.paging__btn(
					v-if="whichPage > 1"
					@click="() => whichPage--"
				) &lt; 
				select.paging__selector
					option.paging__option(
						v-for="i in howMuchPages"
						:selected="i === whichPage"
						@click="() => whichPage = i"
					) {{ i }}
				button.paging__btn(
					v-if="whichPage < howMuchPages"
					@click="() => whichPage++"
				) &gt;
		mixin Modal
			.modal-container(
				v-if="!isDataTypeSelected"
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
						@click="dataTypeSelect"
						value="small"
					) Малый
					button.modal__btn(
						@click="dataTypeSelect"
						value="big"
					) Большой
		mixin Info 
			.info(
				v-if="whichRow >= 0"
			)
				p.info__p Выбран пользователь 
					b {{ cutted[whichRow].firstName + ' ' + cutted[whichRow].lastName }}
				p.info__p Описание: 
				textarea.info__textarea(disabled) {{ cutted[whichRow].description }}
				p.info__p Адрес проживания: 
					b {{ cutted[whichRow].address.streetAddress }}
				p.info__p Город: 
					b {{ cutted[whichRow].address.city }}
				p.info__p Провинция/штат: 
					b {{ cutted[whichRow].address.state }}
				p.info__p Индекс: 
					b {{ cutted[whichRow].address.zip }}

		//! ========== PROD ===========
		.table-data
			+Modal
			+HowMuchData
			+Paging
			
			.search
				input.search__input(
					v-model="searchText"
				)
				button.search__submit(
					@click="filter"
				) &#128269

			details.add-new
				summary.add-new__summary Добавить
				form.add-new__form
					.add-new__row(
						v-for="title in tableHead"
					)
						label.add-new__label {{ title }} :
						input.add-new__input
				button(
					type="submit"
					:disabled="!ableAdding"
				).add-new__button Ок
			
			table.table
				caption.table__caption
				tr.table__row.table__row_header
					td.table__cell №
					th.table__cell.table__cell_header(
						v-for="(colName, key) in tableHead"
						@click="sort(colName, key)"
					) 
						span.sort(v-if="whichSort[key]" v-html="whichSort[key]==='ascend' ? '&uarr;' : '&darr;'")
						| {{ colName }}
				tr.table__row(
					v-for="(person, key) in cutted"
					@click="() => whichRow = key"
				)
					td.table__cell {{ (whichPage - 1) * howMuchData + key }}
					td.table__cell(
						v-for="colName in tableHead"
					) {{ person[colName] }}

			+Paging
			+Info
</template>



<script>
export default {
	name: 'TableData',
	data() {
		return {
			isDataTypeLoading: false,
			isDataTypeSelected: false,
			howMuchSelect: [1, 5, 10, 20, 30, 50,],
			howMuchData: 50,
			whichPage: 1,
			searchText: '',
			ableAdding: false,
			whichRow: Number,
			whichSort: [],
			tableHead: ['id', 'firstName', 'lastName', 'email', 'phone',],
			tableData: [],
			DataCopy: Array,
		}
	},
	computed: {
		tableDataLength() { return this.tableData.length },
		howMuchPages() { return Math.ceil(this.tableDataLength / this.howMuchData) },
		cutted() {
			if (this.tableDataLength <= this.howMuchData) {
				return this.tableData
			} else {
				return this.tableData.slice((this.whichPage - 1) * this.howMuchData, this.whichPage * this.howMuchData)
			}
		}
	},
	methods: {
		dataTypeSelect(e) {
			this.isDataTypeLoading = true
			let requestURL
			e.target.value === "small" ?
				requestURL = "http://www.filltext.com/?rows=32&id={number|1000}&firstName={firstName}&lastName={lastName}&email={email}&phone={phone|(xxx)xxx-xx-xx}&address={addressObject}&description={lorem|32}" :
				requestURL = "http://www.filltext.com/?rows=1000&id={number|1000}&firstName={firstName}&delay=3&lastName={lastName}&email={email}&phone={phone|(xxx)xxx-xx-xx}&address={addressObject}&description={lorem|32}"
			let request = new XMLHttpRequest()
			request.open('GET', requestURL)
			request.responseType = 'json'
			request.send()
			let that = this
			request.onload = () => {
				that.tableData = request.response
				that.DataCopy = request.response
				that.isDataTypeSelected = true
				that.isDataTypeLoading = false
			}
		},
		sort(colName, key) {
			
			let type
			switch (this.whichSort[key]) {
				case undefined:
				case 'discend':
					this.tableData
						.sort((a, b) => {
							if (a[colName] > b[colName]) { return 1 }
							else if (a[colName] < b[colName]) { return -1 }
							else {return (a[0] > b[0])? 1 : -1} // if a === b then sort by "id"
						})
					type = 'ascend'
					break
				case 'ascend':
					this.tableData
						.sort((a, b) => {
							if (a[colName] < b[colName]) { return 1 }
							else if (a[colName] > b[colName]) { return -1 }
							else {return (a[0] < b[0])? 1 : -1} // if a === b then sort by "id"
						})
					type = 'discend'
					break
			}
			this.whichSort = []
			this.whichSort[key] = type
		},
		filter() {
			this.tableData = this.DataCopy.filter(person => {
				let has
				this.tableHead.forEach(key => {
					if(person[key].toString().toLowerCase().startsWith(this.searchText.toString().toLowerCase())) { has = true }
				})
				return has
			})
			let colName
			this.whichSort.forEach(sort => {
				if(sort!==undefined) {
					this.tableData.sort((a, b) => {
						if (a[colName] < b[colName]) { return 1 }
						else if (a[colName] > b[colName]) { return -1 }
						else {return (a[0] < b[0])? 1 : -1} // if a === b then sort by "id"
					})
				}
			})
			// if (cell.toString().toLowerCase().startsWith(event.target.value.toString().toLowerCase())) has = true
			// if (this.tableLen <= (this.pageSelected - 1) * this.quantity) {this.pageSelected = Math.ceil(this.tableLen / this.quantity) || 1}
		}
	},
}
</script>



<style scoped lang="stylus">
</style>

