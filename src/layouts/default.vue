<template>
  <v-app>
    <v-app-bar scroll-behavior="hide" scroll-threshold="500" prominent>
      <template v-slot:prepend>
        <v-app-bar-nav-icon
          variant="text"
          @click.stop="drawer = !drawer"
        ></v-app-bar-nav-icon>
      </template>

      <v-app-bar-title class="title" @click.stop="$router.push('/')"
        >BA Viewer</v-app-bar-title
      >
      <v-spacer></v-spacer>
      <v-spacer></v-spacer>

      <template v-slot:append>
        <!-- <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn> -->

        <v-btn icon @click.stop="$router.go(-1)">
          <v-icon>mdi-arrow-left</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-database</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-cog</v-icon>
        </v-btn>
      </template>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" temporary>
      <v-list density="compact" nav :opened="open">
        <v-list-group
          v-for="([title, tabs], i) in items"
          :value="title"
          :key="i"
        >
          <template v-slot:activator="{ props }">
            <v-list-item v-bind="props" :title="title"></v-list-item>
          </template>

          <v-list-item
            v-for="(tab, j) in tabs"
            :key="j"
            :prepend-icon="tab.icon"
            :title="tab.name"
            :value="tab.name"
            :to="tab.path"
          ></v-list-item>
        </v-list-group>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script lang="ts" setup>
import { ref, watch } from "vue";
import { TABS, type Tab } from "./tabs";

const drawer = ref(false);
const group = ref(null);
const open = ref([]);
const items: Array<[string, Tab[]]> = [
  ["生徒", [TABS.student, TABS.party, TABS.gacha]],
  ["アイテム", [TABS.item, TABS.equipment, TABS.craft, TABS.shop]],
  ["ステージ", [TABS.mission, TABS.bounty, TABS.commissions, TABS.scrimmage]],
  ["イベント", [TABS.event, TABS.guide]],
  [
    "競技",
    [TABS.totalAssault, TABS.jointFiringDrill, TABS.grandAssault, TABS.set],
  ],
  ["モモトーク", [TABS.message, TABS.storyBond, TABS.gift]],
  // ["ストーリー", []],
];
watch(group, () => {
  drawer.value = false;
});
</script>

<style lang="scss" scoped>
.title {
  @apply cursor-pointer;
  flex: 0 1 auto !important;
}
</style>
