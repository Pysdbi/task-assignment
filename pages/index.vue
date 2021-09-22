<template>
	<div class="container">
		<div class="tabs">
			<div class="tabs--list">
				<div
					v-for="(item, ind) in tabs"
					:key="ind"
					class="tabs--list-tab"
					:class="[current_tab === ind ? 'active' : '']"
					@click="onChangeTab(ind)"
				>
					{{ item.title }}
				</div>
			</div>
			<div
				class="tabs--card"
			>
				<component :is="current_tab_comp" />
			</div>
		</div>
	</div>
</template>

<script>
import paramsTab from '~/components/tabs/params-tab'
import questionsTab from '~/components/tabs/questions-tab'
import logicTab from '~/components/tabs/logic-tab'
import conditionsTab from '~/components/tabs/conditions-tab'
import taskTab from '~/components/tabs/tasks-tab'

export default {
	name: 'Index',
	data: () => ({
		tabs: [
			{
				title: 'Параметры',
				component: paramsTab
			},
			{
				title: 'Вопросы',
				component: questionsTab
			},
			{
				title: 'Логика',
				component: logicTab
			},
			{
				title: 'Условия',
				component: conditionsTab
			},
			{
				title: 'Респонденты',
				component: taskTab
			}
		],
		current_tab: 4,
		current_tab_comp: taskTab
	}),

	methods: {
		onChangeTab (ind) {
			this.current_tab = ind
			this.current_tab_comp = this.tabs[ind].component
		}
	}
}
</script>

<style lang="scss">
.container {
	margin: 2rem auto;
	min-height: 100vh;
	width: 100%;
	display: flex;
	min-width: 320px;
	padding-left: 2rem;
	padding-right: 2rem;
}

.tabs {
	display: flex;
	flex-direction: column;
	align-items: center;
	width: 100%;

	.tabs--list {
		display: flex;
		justify-content: space-between;
		align-items: center;
		width: 100%;
		border-bottom: 1.5px solid #C9E55AFF;
		background: white;
		z-index: 2;

		.tabs--list-tab {
			display: flex;
			align-items: center;
			justify-content: center;
			min-height: 48px;
			font-size: 1.2rem;
			width: 100%;
			cursor: pointer;
			color: #2F6D26FF;
			border-bottom: 2px solid transparent;

			&.active {
				color: black;
				font-weight: 600;
				border-bottom: 2px solid #C9E55AFF;
			}
		}
	}

	&--card{
		z-index: 1;
		width: 100%;
		min-height: 400px;
		box-shadow: 0 10px 25px 3px #d7d7d7;
	}
}
</style>
