<template>
  <section class="content-wrapper" style="min-height: 960px;">
    <div class="box-body">
        <back-buttton></back-buttton>
    </div>
    <div class="show-page">
      <!-- <div class="sub-section">
        <div class="sub-heading">
          <i class="fa fa-file-image-o fa-2x" style="display: flex;"><h3 style="margin-left: 10px;">Photos</h3></i>
        </div>
        <div class="sub-content"></div>
      </div> -->
      <hr>
      <div class="sub-section">
        <div class="sub-heading">
          <i class="fa fa-clock-o fa-2x" style="display: flex;"><h3 style="margin-left: 10px;">Timecards</h3></i>
        </div>
        <div class="sub-content">
          <div class="timecard-wrapper" v-for="item in data" v-bind:key="item.userId">
            <Timecard :data="item"/>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import Timecard from './Timecard.vue'

export default {
  data() {
    return {
      getWeekDate: () => {
        const currentDate = moment();
        const weekStart = currentDate.clone().startOf('isoWeek');
        const weekEnd = currentDate.clone().endOf('isoWeek');

        return [moment(weekStart).format("YYYY-MM-DD"), moment(weekEnd).format("YYYY-MM-DD")]
      },
    }
  },
  computed: {
    ...mapGetters('Alert', ['message', 'errors', 'color']),
    ...mapGetters('ProjectsSingle', ['assignedUserList']),
    ...mapGetters('TimecardsIndex', ['data']),
  },
  created() {
    this.fetchData({
      projectId: this.$route.params.id,
      startDate: this.getWeekDate()[0],
      endDate: this.getWeekDate()[1],
    })
  },
  destroyed() {
    this.resetState()
  },
  watch: {
    '$route.params.id': function() {
      this.resetState()
    },
  },
  methods: {
    ...mapActions('Alert', ['resetState']),
    ...mapActions('ProjectsSingle', ['fetchAssignedUserList']),
    ...mapActions('TimecardsIndex', ['fetchData']),
  },
  components: {
    'Timecard': Timecard
  }
}
</script>

<style lang="scss" scoped>
.sub-content {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.show-page {
  padding: 20px;
}
</style>
