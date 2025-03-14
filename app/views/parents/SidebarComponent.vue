<template>
  <div class="sidebar">
    <div class="sidebar__top">
      <div class="sidebar__child">
        <select
          id="sidebar__child__select-id"
          name="child-select"
          class="sidebar__child__select"
          @change="onChildSelectionChange"
        >
          <optgroup>
            <option
              v-for="(child, index) in children"
              :key="index"
              :value="child.userId"
              class="sidebar__child__option"
              :disabled="!child.verified"
              :selected="child.userId === selectedChildrenId"
            >
              {{ child.broadName }} {{ !child.verified ? '(Not Verified)' : '' }}
            </option>
            <option
              v-if="children.length > 0"
              value="No other children added"
              class="sidebar__child__option"
              disabled
            >
              No other children added
            </option>
            <option
              v-if="children.length === 0"
              value="No children added"
              class="sidebar__child__option"
              disabled
              :selected="children.length === 0"
            >
              No children added
            </option>
          </optgroup>
        </select>
      </div>
      <ul class="sidebar__tabs">
        <router-link :to="{ name: 'ParentDashboard', params: { viewName: 'progress', childId: selectedChildrenId, product: product } }">
          <li
            :class="{ sidebar__tabs__item: true, sidebar__tabs__item__sel: selectedTab === 'progress' || selectedTab === 'dashboard' }"
          >
            <img
              src="/images/pages/parents/dashboard/icon-productivity-black.svg"
              alt="Progress"
              class="sidebar__tabs__img"
            >
            <span class="sidebar__tabs__name">Progress</span>
          </li>
        </router-link>
        <router-link :to="{ name: 'ParentDashboard', params: { viewName: 'summary', childId: selectedChildrenId } }">
          <li
            :class="{ sidebar__tabs__item: true, sidebar__tabs__item__sel: selectedTab === 'summary' }"
          >
            <img
              src="/images/pages/parents/dashboard/icon-summary.svg"
              alt="Summary"
              class="sidebar__tabs__img"
            >
            <span class="sidebar__tabs__name">Summary</span>
          </li>
        </router-link>
        <router-link :to="{ name: 'ParentDashboard', params: { viewName: 'online-classes', childId: selectedChildrenId } }">
          <li
            :class="{ sidebar__tabs__item: true, sidebar__tabs__item__sel: selectedTab === 'online-classes' }"
          >
            <img
              src="/images/pages/parents/dashboard/icon-online-classes.svg"
              alt="Online Classes"
              class="sidebar__tabs__img"
            >
            <span class="sidebar__tabs__name">Online Classes</span>
          </li>
        </router-link>
        <router-link
          tag="a"
          to="/roblox"
          target="_blank"
        >
          <li
            :class="{ sidebar__tabs__item: true, sidebar__tabs__item__sel: selectedTab === 'roblox' }"
          >
            <img
              src="/images/ozaria/teachers/dashboard/svg_icons/IconComputer.svg"
              alt="Roblox"
              class="sidebar__tabs__img"
            >
            <span class="sidebar__tabs__name">Roblox</span>
          </li>
        </router-link>
        <router-link
          tag="a"
          to="/ai"
          target="_blank"
        >
          <li
            :class="{ sidebar__tabs__item: true, sidebar__tabs__item__sel: selectedTab === 'ai-hackstack' }"
          >
            <img
              src="/images/ozaria/teachers/dashboard/svg_icons/Icon_Progress_Black.svg"
              alt="AI Hackstack"
              class="sidebar__tabs__img"
            >
            <span class="sidebar__tabs__name"> {{ $t('nav.ai_hackstack') }}</span>
            <span class="beta">({{ $t('nav.beta') }})</span>
          </li>
        </router-link>
        <!--        <router-link :to="{ name: 'ParentDashboard', params: { viewName: 'ai-league', childId: this.selectedChildrenId } }">-->
        <!--          <li-->
        <!--            :class="{ sidebar__tabs__item: true, sidebar__tabs__item__sel: selectedTab === 'ai-league' }"-->
        <!--          >-->
        <!--            <img src="/images/pages/parents/dashboard/icon-ai-league.svg" alt="AI League" class="sidebar__tabs__img">-->
        <!--            <span class="sidebar__tabs__name">AI League</span>-->
        <!--          </li>-->
        <!--        </router-link>-->
      </ul>
      <router-link :to="{ name: 'ParentDashboard', params: { viewName: 'add-another-child' } }">
        <div class="sidebar__add-child">
          <button
            class="sidebar__add-child__btn"
          >
            Add another child
          </button>
        </div>
      </router-link>
    </div>
    <router-link :to="{ name: 'ParentDashboard', params: { viewName: 'toolkit' } }">
      <div class="sidebar__bottom">
        <div
          :class="{ sidebar__bottom__item: true, sidebar__tabs__item__sel: selectedTab === 'toolkit' }"
        >
          Parent toolkit
        </div>
        <!--      <div class="sidebar__bottom__item">Account</div>-->
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'SidebarComponent',
  props: {
    children: {
      type: Array,
      required: true
    },
    defaultTab: {
      type: String
    },
    childId: {
      type: String
    },
    product: {
      type: String
    }
  },
  data () {
    return {
      selectedTab: this.defaultTab || 'progress',
      selectedChildrenId: this.childId ? this.childId : (this.children.length === 0 ? null : this.children[0].userId)
    }
  },
  watch: {
    childId: function (newVal, oldVal) {
      if (newVal !== oldVal) this.selectedChildrenId = this.childId
    },
    defaultTab: function (newVal, oldVal) {
      if (newVal !== oldVal) this.selectedTab = newVal
    }
  },
  methods: {
    onChildSelectionChange (e) {
      this.selectedChildrenId = e.target.value
      this.$emit('onSelectedChildrenChange', this.selectedChildrenId)
    }
  }
}
</script>

<style scoped lang="scss">
@import "./css-mixins/variables";
.sidebar {
  grid-column: sidebar-start / sidebar-end;
  grid-row: 1 / -1;
  border: 1px solid #E6E6E6;
  box-shadow: 0 4px 4px rgba(0, 0, 0, 0.06);
  height: 80vh;

  display: flex;
  flex-direction: column;
  justify-content: space-between;

  &__top {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  &__child {
    align-self: stretch;
    padding-bottom: 1rem;

    &__select {
      width: 100%;
      padding: 1rem;

      font-size: 2rem;
      line-height: 3rem;
      letter-spacing: 0.444444px;

      border: 1px solid #E6E6E6;
      box-shadow: inset 0px -2px 6px rgba(0, 0, 0, 0.15);
    }
  }

  &__tabs {
    list-style: none;
    padding: 0;

    &__item {
      display: flex;
      align-items: center;
      justify-content: flex-start;

      padding: 1rem;

      cursor: pointer;

      &__sel {
        color: $color-twilight !important;
      }
    }

    &__name {
      font-size: 1.8rem;
      line-height: 2.2rem;
      font-weight: 400;

      margin-left: 1rem;
    }

    &__img {
      width: 2.1rem;
      height: 2rem;
    }
  }

  &__add-child {
    &__btn {
      background: $color-yellow-1;
      border-radius: 1px;

      font-weight: 600;
      font-size: 1.6rem;
      line-height: 1.7rem;
      letter-spacing: 0.333333px;
      color: #131B25;

      padding: .5rem 1.5rem;
      border-color: #ffffff;
    }
  }

  &__bottom {
    display: flex;
    align-items: flex-start;
    flex-direction: column;
    padding-bottom: 5rem;
    cursor: pointer;

    &__item {
      align-self: stretch;
      border: 1px solid #E6E6E6;

      font-weight: 600;
      font-size: 1.8rem;
      line-height: 3rem;

      display: flex;
      align-items: center;
      letter-spacing: 0.444444px;
      text-transform: uppercase;
      color: $color-grey-dark-1;
      padding: .5rem;
    }
  }
  a {
    color: inherit;
    text-decoration: none;
  }
  .beta {
    font-size: 12px;
    line-height: 15px;
    position: relative;
    bottom: 5px;
    margin-left: 5px;
  }
}
</style>
