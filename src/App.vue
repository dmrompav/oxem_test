<template lang="pug">
	// *--- DEV ---
	mixin Paging
		Paging(
			:how-much-pages="howMuchPages"
			:which-page="whichPage"
			@which-page="page => whichPage = page"
		)

	// !--- PROD ---
	#app
		Modal(
			:is-data-type-loading="isDataTypeLoading"
			:is-data-type-gotten="isDataTypeGotten"
			@data-type-select="dataTypeSelect"
		)
		HowMuchSel(
			:how-much-select="howMuchSelect"
			:how-much-data="howMuchData"
			@how-much-data="i => howMuchData = i"
		)
		+Paging
		Search(
			@filterData="filterData"
			:table-data-length="tableDataLength"
		)
		AddNewForm(
			:table-head="tableHead"
			:types-head="typesHead"
			:table-ids="tableIds"
			@add-new="addNew"
		)
		TableData(
			@selected-person="person => selectedPerson = person"
			@sort-data-click="sortDataClick"
			:which-page="whichPage"
			:which-sort="whichSort"
			:table-head="tableHead"
			:cutted="cutted"
			:how-much-data="howMuchData"
		)
		+Paging
		Info(
			:selected-person="selectedPerson"
		)
</template>


// ===== SCRIPTS ==============================
<script>
import Modal from './components/Modal.vue'
import HowMuchSel from './components/HowMuchSel.vue'
import Paging from './components/Paging.vue'
import Search from './components/Search.vue'
import AddNewForm from './components/AddNewForm.vue'
import TableData from './components/TableData.vue'
import Info from './components/Info.vue'

export default {
	name: 'App',
	components: {
		Modal, HowMuchSel, Paging, Search, AddNewForm, TableData, Info,
	},
	data() {
		return {
			isDataTypeLoading: false,
			isDataTypeGotten: false,
			howMuchSelect: [1, 5, 10, 20, 30, 50,],
			howMuchData: 50,
			whichPage: 1,
			selectedPerson: undefined,
			searchText: '',
			whichSort: [],
			tableHead: ['id', 'firstName', 'lastName', 'email', 'phone',],
			typesHead: ['text', 'text', 'text', 'email', 'tel'],
			tableData: [],
			DataCopy: [],
			tableIds: [],
		}
	},
	computed: {
		tableDataLength() { return this.tableData.length },
		howMuchPages() { return Math.ceil(this.tableDataLength / this.howMuchData) },
		cutted() {
			if (this.tableDataLength <= this.howMuchData) {return this.tableData} 
			else {return this.tableData.slice((this.whichPage - 1) * this.howMuchData, this.whichPage * this.howMuchData)}
		}
	},
	methods: {
		dataTypeSelect(value) {
			this.isDataTypeLoading = true
			let requestURL
			if(value === "small") {
				requestURL = "https://www.filltext.com/?rows=32&id={number|1000}&firstName={firstName}&lastName={lastName}&email={email}&phone={phone|(xxx)xxx-xx-xx}&address={addressObject}&description={lorem|32}"
			} else {
				requestURL = "https://www.filltext.com/?rows=1000&id={number|1000}&firstName={firstName}&delay=3&lastName={lastName}&email={email}&phone={phone|(xxx)xxx-xx-xx}&address={addressObject}&description={lorem|32}"
			}
			this.dataLoad(requestURL)
				.then(data => {
					this.tableData = this.DataCopy = data
					this.tableData.forEach(e => this.tableIds.push(e.id))
					this.isDataTypeGotten = true
					this.isDataTypeLoading = false
				})
				.catch(err => {
					alert('что-то пошло не так... попробуйте снова')
					console.log(err)
					this.isDataTypeLoading = false
				})
		},
		dataLoad(requestURL) {
			return new Promise((resolve, reject) => {
				const xhr = new XMLHttpRequest()
				xhr.open('GET', requestURL)
				xhr.responseType = 'json'
				xhr.onload = () => {
					if(xhr.status >= 400) {
						reject(xhr.response)
					} else {
						resolve(xhr.response)
					}
				}
				xhr.onerror = () => {
					reject(xhr.response)
				}
				xhr.send()
			})
		},
		sortDataClick(colName, key) {
			switch (this.whichSort[key]) {
				case undefined:
				case 'discend':
					this.whichSort = []
					this.whichSort[key] = 'ascend'
					break
				case 'ascend':
					this.whichSort = []
					this.whichSort[key] = 'discend'
					break
			}
			this.sortData()
		},
		sortData() {
			this.whichSort.forEach((sort, i) => {
				if(sort) {
					let colName = this.tableHead[i]
					switch (sort)  {
						case 'ascend': {
							this.tableData.sort((a, b) => {
								if (a[colName] > b[colName]) { return 1 }
								else if (a[colName] < b[colName]) { return -1 }
								else {return (a[0] > b[0])? 1 : -1} // if a === b then sort by "id"
							})
							break
						}
						case 'discend': {
							this.tableData.sort((a, b) => {
								if (a[colName] < b[colName]) { return 1 }
								else if (a[colName] > b[colName]) { return -1 }
								else {return (a[0] < b[0])? 1 : -1} // if a === b then sort by "id"
							})
							break
						}
					}
				}
			})
		},
		filterData(searchText) {
			this.searchText = searchText
			this.tableData = this.DataCopy.filter(person => {
				let has
				this.tableHead.forEach(key => {
					if(person[key].toString().toLowerCase().includes(searchText.toString().toLowerCase())) { 
						has = true
					}
				})
				return has
			})
			console.log(this.tableData)
			this.sortData()
			console.log(this.tableData)
		},
		addNew(person) {
			console.log('')
			let p = Object.assign({}, person)
			this.DataCopy.push(p)
			console.log(this.DataCopy)
			this.filterData(this.searchText)
		},
	},
}
</script>


// ===== STYLES ==============================
<style lang="stylus">
@import './assets/_reset.styl'
body
	position absolute
	width 100vw
	height 100vh
	background #34495e
	font-family sans-serif
	color #dddddd
	overflow scroll

#app
	position absolute
	
	
</style>

