<template>
  <div class="row">
    <div class="col-12">
      <card :title="table1.title">
        <div class="table-responsive">
          <base-table
            :data="table1.data"
            :columns="table1.columns"
            thead-classes="text-primary"
          >
          </base-table>
        </div>
      </card>
    </div>
  </div>
</template>
<script>
import { accessToken } from '../secrets.js'
import { BaseTable } from "@/components"

const tableColumns = ["Rank", "Guild", "Server"]

export default {
  components: {
    BaseTable
  },
  data () {
    return {
      tableData: [],
      table1: this.createTable("Loading...", this.tableData),
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
    const url = 'https://eu.api.blizzard.com/data/wow/leaderboard/hall-of-fame/uldir/horde?namespace=dynamic-EU&locale=en_EU&access_token=' + accessToken
    this.$axios.get(url).then(result => {
      console.log(result)
      let tableData = result.data.entries.map(entry => {
        return {
          rank: entry.rank,
          guild: entry.guild.name,
          server: entry.guild.realm.slug,
        }
      })
      this.table1 = this.createTable("Simple Table", tableData)
    }).catch(err => console.error(err))
  }
}
</script>
<style>
</style>
