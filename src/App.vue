<template>
  <div id="app">
    <div class="row">
      <div>
        <div>Applications Facet Filter State</div>
        <pre class="cell shadow">{{facetFilterState.applications}}</pre>
        <div>
          {{applications.length}} Application{{applications.length === 1 ? '' : 's'}}
        </div>
      </div>
      <div>
        <div>UserGroups Facet Filter State</div>
        <pre class="cell shadow">{{facetFilterState.userGroups}}</pre>
        <div>
          {{userGroups.length}} User Group{{userGroups.length === 1 ? '' : 's'}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
export default {
  name: 'app',
  data: () => ({
    facetFilterState: { applications: {}, userGroups: {} },
    applications: [],
    userGroups: []
  }),
  created () {
    this.$lx.init()
      .then(reportSetup => {
        console.log('REPORT SETUP', reportSetup)
        const reportConfig = {
          facets: [
            {
              key: 1,
              label: 'Applications',
              fixedFactSheetType: 'Application',
              facetFiltersChangedCallback: filter => Vue.set(this.facetFilterState, 'applications', filter),
              callback: dataset => { this.applications = dataset }
            },
            {
              key: 2,
              label: 'User Groups',
              fixedFactSheetType: 'UserGroup',
              facetFiltersChangedCallback: filter => Vue.set(this.facetFilterState, 'userGroups', filter),
              callback: dataset => { this.userGroups = dataset }
            }
          ]
        }
        this.$lx.ready(reportConfig)
      })
  }
}
</script>
<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

.row
  display flex
  align-items center
  justify-content space-around
  margin 1rem
  padding 1rem
  border 1px solid black
.cell
  padding 1rem
  margin 1rem

.shadow
  box-shadow 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23)
</style>
