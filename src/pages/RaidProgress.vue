<template>
  <div class="row">
    <div class="col-12">
      <card :title="raidTable.title">
        <div class="table-responsive">
          <base-table
            :data="raidTable.data"
            :columns="raidTable.columns"
            thead-classes="text-primary"
          >
          </base-table>
        </div>
      </card>
    </div>
  </div>
</template>
<script>
import { BaseTable } from "@/components"

// Column header names
const tableColumns = ["Rank", "Guild", "Faction", "Realm", "Progress"]

export default {
  components: {
    BaseTable
  },
  data () {
    return {
      raidTable: this.createTable("Loading...", []),
    }
  },
  methods: {
    createTable (title, data) {
      return {
        title,
        columns: tableColumns,
        data
      }
    }
  },
  created () {
    const url = 'https://raider.io/api/v1/raiding/raid-rankings?raid=nyalotha-the-waking-city&difficulty=Mythic&region=eu&realm=Twisting%20Nether'
    const urlCors = 'https://cors-anywhere.herokuapp.com/' + url
    let numRaids = 12

    this.$axios.get(urlCors, {
      crossdomain: true,
      headers: {
        'origin': 'http://le-progress.com',
        'Access-Control-Allow-Origin': '*',
        'Access-Control-Allow-Headers': 'Content-Type'
      }
    }).then(result => {
      console.log(result)
      let tableData = result.data.raidRankings.map(entry => {
        // Row name: row value
        return {
          rank: entry.rank,
          guild: entry.guild.name,
          faction: entry.guild.faction,
          progress: entry.encountersDefeated.length + '/' + numRaids,
          realm: entry.guild.realm.name,
        }
      })
      this.raidTable = this.createTable("Raid Progress", tableData)
    }).catch(err => console.error(err))
  }
}
</script>
<style>
</style>
