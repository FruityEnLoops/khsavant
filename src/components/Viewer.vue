<script setup>
const props = defineProps(['game', 'achievementData', 'gameStats']);
import { CTable, CTableHead, CTableRow, CTableHeaderCell, CTableBody, CTableDataCell } from '@coreui/vue';
</script>

<template>
    <div>
        <p>Current progression : {{ getAchievementsForCurrentGame(game, achievementData, gameStats).filter((ach) => ach.achieved === 1).length }} / {{ game.achievements.length }}</p>
        <CTable>
            <CTableHead>
                <CTableRow>
                <CTableHeaderCell scope="col">Icon</CTableHeaderCell>
                <CTableHeaderCell scope="col">Name</CTableHeaderCell>
                <CTableHeaderCell scope="col">Description</CTableHeaderCell>
                <CTableHeaderCell scope="col">Unlocked</CTableHeaderCell>
                </CTableRow>
            </CTableHead>
            <!-- <div v-for="ach of getAchievementsForCurrentGame(game, achievementData, gameStats)">
                <Achievement :achievementName="ach.name" :achievementDesc="ach.description" :unlocked="ach.achieved === 1" :icon="ach.achieved === 1 ? ach.icon : ach.icongray"/>
            </div> -->
            <CTableBody v-for="ach of getAchievementsForCurrentGame(game, achievementData, gameStats)" class="rowstyle">
                <CTableDataCell><img :src="ach.achieved === 1 ? ach.icon : ach.icongray" width="64" height-="64"></CTableDataCell>
                <CTableDataCell>{{ ach.name }}</CTableDataCell>
                <CTableDataCell>{{ ach.description }}</CTableDataCell>
                <CTableDataCell>{{ ach.achieved === 1 ? "✅" : "❌" }}</CTableDataCell>
            </CTableBody>
        </CTable>
    </div>
</template>

<script>
export default {
    methods: {
        getAchievementsForCurrentGame(game, achievementData, gameStats) {
            const list = [];
            const jsonRes = achievementData.find((res) => res.appid === game.appId); // finds the appropriate data from game's appid
            const statsForApps = gameStats.find((res) => res.appid === game.appId);
            if(!jsonRes || !statsForApps) {
                return list;
            }
            for(const ach of game.achievements) {
                // match the game's achievement with steam's api
                let matched = jsonRes.achievements.find((resAchievement) => resAchievement.apiname === ach);
                // inject icons from gameStats
                let metaMatch = statsForApps.gameStats.find((stat) => stat.name === ach);
                matched.icon = metaMatch.icon;
                matched.icongray = metaMatch.icongray;
                list.push(matched);
            }
            return list;
        }
    }
}
</script>

<style scoped>
.rowstyle {
    vertical-align: middle;
    text-align: center;
}

p {
    text-align: center;
}
</style>