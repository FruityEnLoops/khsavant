<script setup>
import Viewer from './components/Viewer.vue';
import infoJson from './infoJson.json';
import { CAccordion, CAccordionItem, CAccordionBody, CAccordionHeader } from '@coreui/vue'
</script>

<template>
	<header :class="{ hidden: hideForm }">
		<img alt="KHSteamAchievementViewer" class="logo" src="./assets/logo.png" width="128" height="128" />
		<p>KHSavant : Steam Achievement Viewer</p>
	</header>
	<main>
		<div class="authForm" :class="{ hidden: hideForm }">
			<p>Steam Profile ID : <a href="https://steamid.io/">(get here)</a></p>
			<input v-model="steamid" placeholder="Insert your SteamID" />
			<p><button @click="go">Go!</button></p>
		</div>
		<div class="viewer" :class="{ hidden: !hideForm }">
			<p class="creditsviewer">Images from <a href="https://github.com/Televo/kingdom-hearts-recollection">KH
					Re:Collection by Televo</a></p>
			<CAccordion flush class="acc">
				<CAccordionItem v-for="game in infoJson">
					<CAccordionHeader>
						<img :src="`/src/assets/games/${game.assetName}`" width="32" height="32" class="gameLogo"/> {{ game.appName }}
					</CAccordionHeader>
					<CAccordionBody>
						<Viewer :game="game" :achievementData="achievementData" :gameStats="gameStats"/>
					</CAccordionBody>
				</CAccordionItem>
			</CAccordion>
		</div>
	</main>
	<p class="creditsmain" :class="{ hidden: hideForm }">Images from <a
			href="https://github.com/Televo/kingdom-hearts-recollection">KH Re:Collection by Televo</a></p>
</template>

<script>
export default {
	achievementInfo: infoJson,
	components: {},
	data() {
		return {
			steamid: '',
			hideForm: false,
			achievementData: [],
			gameStats: []
		}
	},
	mounted() {
		this.steamid = localStorage.getItem('steamid') !== null ? window.localStorage.getItem('steamid') : '';
	},
	methods: {
		go(event) {
			if (
				this.steamid != '' &&
				this.steamid.length == 17
			) {
				this.hideForm = true;
				// store steamid in browser localStorage
				localStorage.setItem('steamid', this.steamid);

				// retrieve data from steam api
				const appIds = ["2552430", "2552440", "2552450"] // corresponds to 1.5+2.5, 2.8 and KH3 respectively.
				for (const appId of appIds) {
					fetch(`/khsavant/proxy/GetPlayerAchievements?appid=${appId}&steamid=${this.steamid}`)
						.then((response) => {
							response.json().then((data) => {
								this.achievementData.push({ appname: data.playerstats.gameName, appid: appId, achievements: data.playerstats.achievements })
							});
						})
						.catch((err) => {
							console.error(err);
						});
					fetch(`/khsavant/proxy/GetSchemaForGame?appid=${appId}`)
						.then((response) => {
							response.json().then((data) => {
								this.gameStats.push({ appid: appId, gameStats: data.game.availableGameStats.achievements });
							})
						})
						.catch((err) => {
							console.error(err);
						});
				}
			}
		}
	}
}
</script>

<style scoped>

.viewer {
	width: 80%;
	min-width: 80%;
	margin-left: 10%;
	max-width: 80%;
	margin-right: 10%;
	display: flexbox;
}

.gameLogo {
	margin-right: 15px;
}

.creditsviewer {
	text-align: center;
	margin-bottom: 20px;
}

.creditsmain {
	margin-top: -10px;
	margin-left: 20%;
}

main {
	width: 200%;
	min-width: 100%;
}

p {
	margin-top: 15px;
	margin-bottom: 5px;
}

button {
	margin-left: 15%;
	width: 5%;
	border: none;
	background-color: black;
	color: white;
}

.logo {
	display: block;
	margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
	header {
		line-height: 1.5;
	}

	header {
		display: flex;
		flex-wrap: wrap;
		place-items: center;
		padding-right: calc(var(--section-gap) / 2);
	}

	.logo {
		margin: 0 2rem 0 0;
	}

	header .wrapper {
		display: flex;
		place-items: flex-start;
		flex-wrap: wrap;
	}
}

input {
	width: 35%;
}

.hidden {
	display: none;
}
</style>