<template>
	<div class="app">
		<div class="app-drawer">
			drawer
		</div>
		<main class="app-main">
			<header class="app-bar">
				<div class="app-bar--search">
					<input
						type="text"
						class="app-bar--search-input"
						placeholder="Поиск по системе"
					>
					<div
						class="app-bar--search-icon"
					>
						<img src="~/assets/imgs/search.svg" alt="search-icon">
					</div>
				</div>
				<div class="app-bar--profile">
					<div
						class="app-bar--profile-icon"
					>
						<img
							src="~/assets/imgs/profile.svg"
							alt="profile-icon"
						>
					</div>
					<div class="app-bar--profile-info">
						<div style="font-size: 1.25rem;font-weight: 500">
							Alexander C.
						</div>
						<div style="color: #8d8d8d;font-size: .9rem">
							Администратор
						</div>
					</div>
				</div>
			</header>
			<Nuxt />
		</main>
	</div>
</template>

<script>
export default {
	name: 'Default'
}
</script>

<style lang="scss">
html {
	font-family: 'Source Sans Pro',
	-apple-system,
	BlinkMacSystemFont,
	'Segoe UI',
	Roboto,
	'Helvetica Neue',
	Arial,
	sans-serif;
	font-size: 16px;
	word-spacing: 1px;
	-ms-text-size-adjust: 100%;
	-webkit-text-size-adjust: 100%;
	-moz-osx-font-smoothing: grayscale;
	-webkit-font-smoothing: antialiased;
	box-sizing: border-box;
}

*,
*::before,
*::after {
	box-sizing: border-box;
	margin: 0;
}

.app {
	position: relative;
	display: flex;
	justify-content: stretch;

	.app-drawer {
		position: sticky;
		width: 150px;
		z-index: 1000;
		border-right: .8px solid #c6c6c6;
	}

	.app-main {
		width: 100%;

		.app-bar {
			min-height: 4.75rem;
			width: 100%;
			display: flex;
			justify-content: space-between;
			padding-left: 2rem;
			padding-right: 2rem;
			align-items: center;

			&--search {
				position: relative;
				width: 620px;
				height: 48px;
				display: flex;

				&-input {
					width: 100%;
					border: 1px solid #c6c6c6;
					border-right: none;
					height: 48px;
					border-radius: 6px 0 0 6px;
					outline: none;
					color: #9DBFAF;
					font-size: 100%;
					padding-left: 1rem;

					&:focus {
						color: black;
					}
				}

				&-icon {
					width: 40px;
					height: 48px;
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

			&--profile {
				display: flex;
				align-items: center;
				justify-content: center;

				&-icon{
					background: #eeeeee;
					border-radius: 50px;
					width: 56px;
					height: 56px;
					display: flex;
					align-items: center;
					justify-content: center;
					img{
						width: 48px;
					}
				}

				&-info{
					margin-left: .5rem;
				}
			}
		}
	}
}
</style>
