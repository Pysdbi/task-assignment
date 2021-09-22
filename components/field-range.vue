<template>
	<div class="field-range">
		<span style="margin-right: .5rem;">
			от
		</span>
		<div class="field field-from">
			<input
				v-model="value_from"
				type="text"
				class="field-input"
				placeholder="от"
				@input="onChangeInput"
			>
		</div>
		<span style="margin-right: .5rem;">
			до
		</span>
		<div class="field field-to">
			<input
				v-model="value_to"
				type="text"
				class="field-input"
				placeholder="до"
				@input="onChangeInput"
			>
		</div>
	</div>
</template>

<script>
export default {
	name: 'FieldRange',
	props: {
		fieldValue: {
			type: Object,
			default: null
		}
	},

	data: () => ({
		value_from: null,
		value_to: null
	}),

	methods: {
		onChangeInput ({ event }) {
			// eslint-disable-next-line vue/no-mutating-props
			this.fieldValue.to = this.value_to
			// eslint-disable-next-line vue/no-mutating-props
			this.fieldValue.from = this.value_from
		}
	}
}
</script>

<style scoped lang="scss">

.field-range {
	display: flex;
	align-items: center;
}

.field-from {
	margin-right: 2rem;
}

.field {
	position: relative;
	width: 200px;
	height: 48px;
	display: flex;

	&-input {
		width: 100%;
		border: 1px solid #c6c6c6;
		height: 48px;
		border-radius: 6px;
		outline: none;
		color: #9DBFAF;
		font-size: 100%;
		padding-left: 1rem;
		transition: all .5s ease;

		&:focus {
			color: black;
		}
	}
}
</style>
