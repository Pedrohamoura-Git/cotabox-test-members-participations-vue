<template>
  <transition
    v-if="membersList.length == 0"
    enter-active-class="fade-in"
    leave-active-class="fade-out"
    mode="out-in"
    appear
  >
    <section class="add-data-msg">
      <h1 class="cart-title">
        Seems like it's gonna be a lonely night... T.T<br />
        Add a new member so I can have some company.
      </h1>
    </section>
  </transition>
  <transition
    v-else
    enter-active-class="fade-in"
    leave-active-class="fade-out"
    mode="out-in"
    appear
  >
    <section class="members-data">
      <div class="members-data__about">
        <h1 class="card-title">Data</h1>
        <p>Lorem ipsum dolor sit, amet consectetur adipisicing elit. Eaque, earum.</p>
      </div>
      <transition
        enter-active-class="slide-in-left"
        leave-active-class="slide-out-left"
        mode="out-in"
        appear
      >
        <TableData v-if="membersList.length != 0" />
      </transition>
      <transition
        enter-active-class="scale-in-center"
        leave-active-class="scale-out-center"
        mode="out-in"
        appear
      >
        <vc-donut
          v-if="sections.length != 0"
          background="white"
          foreground="lightgrey"
          :size="200"
          unit="px"
          :thickness="40"
          has-legend
          legend-placement="right"
          :sections="sections"
          :total="100"
          :start-angle="0"
          :auto-adjust-text-size="true"
        >
          <h1 style="color: black">Total: <br />{{ 100 - availableParticipation }}%</h1>
        </vc-donut>
      </transition>
    </section>
  </transition>
</template>
<script>
import TableData from '../TableData/TableData'

import { isLocalStorageEmpty, getInLocalStorage } from '../../utils/localStorage/index'

export default {
  components: {
    TableData,
  },
  mounted() {
    this.handleItemsInLocalStorage()
  },
  computed: {
    membersList() {
      return this.$store.state.membersList.membersList
    },
    availableParticipation() {
      return this.$store.state.membersList.availableParticipation
    },
    sections() {
      return this.$store.state.chartData.sections
    },
    participationsList() {
      const participationsList = this.membersList.map((member) => member.participation)
      return participationsList
    },
  },
  methods: {
    handleItemsInLocalStorage() {
      this.checkMembersList()
      this.checkSections()
    },
    checkMembersList() {
      if (isLocalStorageEmpty('membersList') == false && this.membersList.length == 0) {
        const membersList = getInLocalStorage('membersList')
        this.ADD_ALL_MEMBERS(membersList)
        this.UPDATE_AVAILABLE_PARTICIPATION()
      }
    },
    ADD_ALL_MEMBERS(membersList) {
      this.$store.dispatch('membersList/ADD_ALL_MEMBERS', membersList)
    },
    UPDATE_AVAILABLE_PARTICIPATION() {
      this.$store.dispatch('membersList/UPDATE_AVAILABLE_PARTICIPATION')
    },
    checkSections() {
      if (isLocalStorageEmpty('sections') == false && this.sections.length == 0) {
        const sections = getInLocalStorage('sections')
        this.ADD_ALL_SECTIONS(sections)
      }
    },
    ADD_ALL_SECTIONS(member) {
      this.$store.dispatch('chartData/ADD_ALL_SECTIONS', member)
    },
  },
}
</script>
<style lang="scss" src="./_membersData.scss" scoped />
