<template>
	<div class="tasks-tab">
		<div class="tasks-tab--title">
			Добавить опрос
		</div>
		<div
			v-for="(item, ind) in tasks"
			:key="ind"
			class="tasks-tab--clause"
			:class="{'alt': ind % 2}"
		>
			<div
				v-show="ind > 0"
				class="tasks-tab--clause-tag-and"
			>
				И
			</div>

			<div class="tasks-tab--clause-row">
				<div class="tasks-tab--clause-title">
					{{ item.name }}
				</div>
				<div class="tasks-tab--clause-types">
					<field-select
						style="width: 600px"
						:current-item="item.type"
						:list-items="getListTaskTypes"
						:item-func="(x)=>x"
						:parent-name="item.name"
						@onSelect="updateTaskType"
					/>
				</div>
			</div>
			<div
				class="tasks-tab--clause-fields"
			>
				<div
					v-for="(item_field, ind_field) in item.fields"
					:key="ind_field"
					class="tasks-tab--clause-fields-field"
				>
					<div
						class="tasks-tab--clause-fields-field-name"
					>
						<div
							v-show="ind_field > 0"
							class="tasks-tab--clause-fields-field-name-or"
						>
							или
						</div>
						{{ item.fieldName }} {{ ind_field + 1 }}
					</div>
					<div
						v-show="item.taskType"
						class="tasks-tab--clause-fields-field-field"
					>
						<field-range
							v-if="item.fieldType === 'range'"
							:field-value="item_field"
						/>
						<field-select
							v-if="item.fieldType === 'select'"
							:current-item="item_field"
							:list-items="getListValuesTaskTypes(item.taskType)"
							:item-func="(x)=>x.title"
							:is-active-func="(_arr, _item, _itemSearch)=> (_arr[_item.id].title === _itemSearch)"
						/>
					</div>
				</div>
				<div class="tasks-tab--clause-actions">
					<div
						v-if="item.type"
						class="btn-add"
						@click="addCondTask(ind)"
					>
						Добавить {{ item.fieldName.toLowerCase() }}
					</div>
					<div
						class="btn-remove"
						@click="removeCondTask(ind)"
					>
						Удалить условие
					</div>
				</div>
			</div>
		</div>
		<div
			class="tasks-tab--add-task"
			@click="addNewTask"
		>
			<span style="text-align: center;font-size: 2rem">+</span>
			Нажмите, чтобы добавить новое условие выборки.
			<br>
			Все условия связываются между собой логическим "И"
		</div>

		<div class="tasks-tab--action">
			<div
				class="btn-save"
				@click="saveTasks"
			>
				Сохранить
			</div>
		</div>
	</div>
</template>

<script>
// TODO Можно было сделать более качественную структуру, но время ограничено

const taskTypes = {
	Age: 'Возраст респондента',
	TypeLoyaltyCard: 'Тип карты лояльности',
	StatusLoyaltyCard: 'Статус карты лояльности'
}
const taskTypesAlt = {
	'Возраст респондента': 'Age',
	'Тип карты лояльности': 'TypeLoyaltyCard',
	'Статус карты лояльности': 'StatusLoyaltyCard'
}
const taskTypesListValues = {
	TypeLoyaltyCard: [
		{
			id: 0,
			title: 'Standard'
		},
		{
			id: 1,
			title: 'Gold'
		},
		{
			id: 2,
			title: 'Platinum'
		}
	],
	StatusLoyaltyCard: [
		{
			id: 0,
			title: 'Активна'
		},
		{
			id: 1,
			title: 'Не активна'
		}
	]
}
const taskFieldsName = {
	Age: 'Диапозон',
	TypeLoyaltyCard: 'Тип',
	StatusLoyaltyCard: 'Статус'
}
const fieldTypes = {
	range: 'range',
	select: 'select'
}

export default {
	name: 'TasksTab',

	data: () => ({
		tasks: [
			{
				name: 'Условие 1',
				taskType: 'Age',
				type: taskTypes.Age,
				fieldName: taskFieldsName.Age,
				fieldType: fieldTypes.range,
				fields: [
					{
						from: null,
						to: null
					},
					{
						from: null,
						to: null
					}
				]
			},
			{
				name: 'Условие 2',
				taskType: 'TypeLoyaltyCard',
				type: taskTypes.TypeLoyaltyCard,
				fieldName: taskFieldsName.TypeLoyaltyCard,
				fieldType: fieldTypes.select,
				fields: [
					'Gold'
				]
			},
			{
				name: 'Условие 3',
				taskType: 'StatusLoyaltyCard',
				type: taskTypes.StatusLoyaltyCard,
				fieldName: taskFieldsName.StatusLoyaltyCard,
				fieldType: fieldTypes.select,
				fields: [
					'Активна'
				]
			}
		]
	}),

	computed: {
		getListTaskTypes () {
			return taskTypes
		}
	},

	methods: {
		getListValuesTaskTypes (taskType) {
			return taskTypesListValues[taskType]
		},

		updateTaskType (itemName, type) {
			const newTasks = []
			this.tasks.forEach((x) => {
				const fieldNameDict = {
					Age: 'range',
					TypeLoyaltyCard: 'select',
					StatusLoyaltyCard: 'select'
				}
				const _name = x.name
				let _taskType
				let _fields
				if (x.name === itemName) {
					_taskType = taskTypesAlt[type]
					_fields = []
				} else {
					_taskType = x.taskType
					_fields = x.fields
				}

				x = {
					name: _name,
					taskType: _taskType,
					type: taskTypes[_taskType],
					fieldName: taskFieldsName[_taskType],
					fieldType: fieldTypes[fieldNameDict[_taskType]],
					fields: _fields
				}

				newTasks.push(x)
			})
			this.tasks = newTasks
		},

		addCondTask (index) {
			// TODO не костыль, а заглушка
			let defaultField
			if (this.tasks[index].taskType === 'Age') {
				defaultField = {
					from: null,
					to: null
				}
			} else {
				defaultField = ''
			}

			this.tasks[index].fields.push(defaultField)
		},

		removeCondTask (index) {
			const temp = []
			this.tasks.forEach((item, ind) => {
				if (ind !== index) {
					temp.push(item)
				}
			})
			this.tasks = temp
		},

		addNewTask () {
			this.tasks.push({
				name: 'Условие ' + (this.tasks.length + 1),
				taskType: null,
				type: null,
				fieldName: null,
				fieldType: null,
				fields: []
			})
		},

		saveTasks () {
			// TODO нужно выносить в store (dispatch)
			this.$axios.post('http://localhost:5000/api/create-task', JSON.stringify(this.tasks))
		}
	}
}
</script>

<style scoped lang="scss">

.tasks-tab {
	&--title {
		color: #8d8d8d;
		font-size: large;
		font-weight: 600;
		padding: 1.5rem;
	}

	&--clause {
		position: relative;
		min-height: 150px;
		width: 100%;
		background: #FFFCF6FF;
		display: flex;
		flex-direction: column;
		padding: 1rem 2.5rem;

		&-tag-and {
			position: absolute;
			top: -20px;
			width: 60px;
			height: 40px;
			background: #ffecc8;
			border-radius: 9px;
			padding: 10px;
			display: flex;
			align-items: center;
			justify-content: center;
		}

		&-row {
			display: flex;
			align-items: center;
			justify-content: space-between;
			min-height: 60px;
		}

		&-title {
			color: #9c7325;
			font-size: x-large;
		}

		&-fields {
			&-field {
				min-height: 60px;
				display: flex;
				justify-content: space-between;
				align-items: center;

				&-name {
					display: grid;
					align-items: center;
					grid-template-columns: repeat(2, 1fr);
					grid-gap: 5px;

					&-or {
						width: 65px;
						background: rgba(156, 115, 37, .1);
						border-radius: 9px;
						padding: 10px;
						text-align: center;
					}
				}
			}
		}

		&-actions {
			margin-top: 2rem;
			min-height: 60px;
			display: flex;
			justify-content: flex-end;
			align-items: center;

			.btn-add {
				border-radius: 6px;
				border: 2px solid #C9E55AFF;
				color: #2F6D26FF;
				padding: 10px 25px;
				margin-right: 2.5rem;
				background: #fff;
				cursor: pointer;
				transition: all .2s ease;

				&:hover {
					background: #f6fcf2;
				}
			}

			.btn-remove {
				border-radius: 6px;
				border: 2px solid darkred;
				color: red;
				padding: 10px 25px;
				background: #fff;
				cursor: pointer;
				transition: all .2s ease;

				&:hover {
					background: #fcf2f2;
				}
			}
		}

		&.alt {
			background: #F8FAFEFF;

			.tasks-tab--clause-tag-and {
				background: #dfe9ff;
			}

			.tasks-tab--clause-title {
				color: #294aa0;
			}

			.tasks-tab--clause-fields-field-name-or {
				background: rgba(#294aa0, .1);
			}
		}
	}

	&--add-task {
		margin: 2rem;
		border: 2px solid #c6c6c6;
		border-radius: 6px;
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		min-height: 120px;
		font-size: 1.1rem;
		color: #2F6D26FF;
		padding-bottom: 1rem;
		cursor: pointer;
		transition: all .2s ease;

		&:hover {
			background: #f0ffe4;
		}
	}

	&--action {
		width: 100%;
		display: flex;
		justify-content: flex-end;
		align-items: center;
		margin-bottom: 2rem;

		.btn-save {
			border-radius: 6px;
			background: #5ba652;
			color: #fff;
			padding: 10px 25px;
			margin-right: 2rem;
			cursor: pointer;
			transition: all .2s ease;

			&:hover {
				background: #60af57;
			}
		}
	}
}
</style>
