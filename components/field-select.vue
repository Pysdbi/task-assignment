<template>
	<div class="field-select">
		<div
			class="field"
			@click="visibleMenu=!visibleMenu"
		>
			<div
				class="field-input"
				v-text="currentItem || 'Выберите'"
			/>
			<div
				class="field-icon"
			>
				<img width="16" src="~/assets/imgs/arrow.svg" alt="arrow-icon">
			</div>
		</div>
		<div
			v-show="visibleMenu"
			class="field-select-menu"
		>
			<div
				v-for="(item, ind) in listItems"
				:key="ind"
				class="field-select-menu-item"
				:class="{active: isActiveFunc(listItems, item, currentItem)}"
				@click="selectItem(itemFunc(item))"
				v-text="itemFunc(item)"
			/>
		</div>
	</div>
</template>

<script>
export default {
	name: 'FieldSelect',
	props: {
		currentItem: {
			type: Object,
			default: null
		},
		listItems: {
			type: Array,
			default: null
		},
		itemFunc: {
			type: Function,
			default: () => ({})
		},
		isActiveFunc: {
			type: Function,
			default: () => false
		},
		parentName: {
			type: String,
			default: ''
		}
	},
	emits: ['onSelect'],

	data: () => ({
		visibleMenu: false
	}),

	methods: {
		selectItem (item) {
			// eslint-disable-next-line vue/no-mutating-props
			this.currentItem = item
			this.visibleMenu = false
			this.$emit('onSelect', this.parentName, item)
		}
	}
}
</script>

<style scoped lang="scss">
.field-select {
	position: relative;
	width: 320px;
	height: 48px;

	&-menu {
		position: absolute;
		left: 0;
		top: 0;
		width: 100%;
		background: #fff;
		box-shadow: 0 5px 10px 1px #d2d0d0;
		border-radius: 6px;
		padding: 5px 0;
		z-index: 1000;

		&-item {
			width: 100%;
			cursor: pointer;
			transition: all .2s ease;
			padding: 15px 10px;

			&:hover {
				background: #ececec;
			}

			&.active {
				background: #edf4ff;
			}
		}
	}
}

.field {
	width: 100%;
	height: 100%;
	display: flex;
	cursor: pointer;

	&-input {
		width: 100%;
		border: 1px solid #c6c6c6;
		background: #fff;
		border-right: none;
		height: 48px;
		border-radius: 6px 0 0 6px;
		font-size: 100%;
		padding-left: 1rem;
		display: flex;
		align-items: center;
		justify-content: flex-start;
	}

	&-icon {
		width: 40px;
		height: 48px;
		background: #fff;
		border: 1px solid #c6c6c6;
		border-left: none;
		border-radius: 0 6px 6px 0;
		cursor: default;
		display: flex;
		align-items: center;
		justify-content: center;
		padding-right: 1rem;
	}
}
</style>
