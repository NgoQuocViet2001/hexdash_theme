<script setup lang="ts">
import { computed, reactive, ref, toRefs, watch, watchEffect, defineComponent } from 'vue';
//import type { PropType } from 'vue';
import { useStore } from 'vuex';
import { useRoute } from 'vue-router';
import versions from '../demoData/changelog.json';
import { NavTitle } from './style';

const props = defineProps({
  toggleCollapsed: {
    type: Function,
    required: true,
  },
  events: {
    type: Object,
    required: true,
  },
});

const store = useStore();
const darkMode = computed(() => store.state.themeLayout.data);
const mode = ref('inline');
const { events } = toRefs(props);
const { onRtlChange, onLtrChange, modeChangeDark, modeChangeLight, modeChangeTopNav, modeChangeSideNav } = events.value;

const router = computed(() => useRoute());
const state = reactive({
  rootSubmenuKeys: ['sub1', 'sub2', 'sub4'],
  selectedKeys: ['home'],
  openKeys: ['dashboard'],
  preOpenKeys: ['dashboard'],
});

const onOpenChange = (keys: any) => {
  state.openKeys = keys[keys.length - 1] !== 'recharts' ? [keys.length && keys[keys.length - 1]] : keys;
};

const onClick = (item: { keyPath: any }) => {
  if (item.keyPath.length === 1) state.openKeys = [];
};

watchEffect(() => {
  if (router.value.matched.length) {
    if (router.value.matched.length > 2) {
      state.selectedKeys = [router.value.matched[2]?.name].filter((x) => x !== undefined) as string[];
      state.openKeys = [router.value.matched[1]?.name].filter((x) => x !== undefined) as string[];
      state.preOpenKeys = [router.value.matched[1]?.name].filter((x) => x !== undefined) as string[];
    } else if (router.value.matched.length > 3) {
      state.selectedKeys = [router.value.matched[3]?.name].filter((x) => x !== undefined) as string[];
      state.openKeys = [router.value.matched[1]?.name].filter((x) => x !== undefined) as string[];
      state.preOpenKeys = [router.value.matched[1]?.name].filter((x) => x !== undefined) as string[];
    } else {
      state.selectedKeys = [router.value.matched[1]?.name].filter((x) => x !== undefined) as string[];
      state.openKeys = [router.value.matched[1]?.name].filter((x) => x !== undefined) as string[];
      state.preOpenKeys = [router.value.matched[1]?.name].filter((x) => x !== undefined) as string[];
    }
  }
});

watch(
  () => state.openKeys,
  (val, oldVal) => {
    state.preOpenKeys = oldVal;
  },
);
</script>

<template>
  <a-menu
    :open-keys="state.openKeys"
    v-model:selectedKeys="state.selectedKeys"
    :mode="mode"
    :theme="darkMode ? 'dark' : 'light'"
    class="scroll-menu"
    @openChange="onOpenChange"
    @click="onClick"
  >
    <a-sub-menu key="layout">
      <template #icon>
        <unicon name="window-section"></unicon>
      </template>
      <template #title>Layouts</template>
      <a-menu-item @click="toggleCollapsed" key="topMenu">
        <a
          @click="
            (e) => {
              e.preventDefault();
              toggleCollapsed();
              modeChangeTopNav();
            }
          "
          to="#"
        >
          Top Menu
        </a>
      </a-menu-item>
      <a-menu-item @click="toggleCollapsed" key="sideMenu">
        <a
          @click="
            (e) => {
              e.preventDefault();
              toggleCollapsed();
              modeChangeSideNav();
            }
          "
          to="#"
        >
          Side Menu
        </a>
      </a-menu-item>
      <a-menu-item @click="toggleCollapsed" key="rtl">
        <a
          @click="
            (e) => {
              e.preventDefault();
              toggleCollapsed();
              onRtlChange();
            }
          "
          to="#"
        >
          RTL
        </a>
      </a-menu-item>
      <a-menu-item @click="toggleCollapsed" key="ltr">
        <a
          @click="
            (e) => {
              e.preventDefault();
              toggleCollapsed();
              onLtrChange();
            }
          "
          to="#"
        >
          LTR
        </a>
      </a-menu-item>
    </a-sub-menu>

    <a-menu-item @click="toggleCollapsed" key="changelog">
      <template #icon>
        <unicon name="arrow-growth"></unicon>
      </template>
      <router-link to="/changelog">
        Changelog
        <span class="badge badge-primary menuItem">{{ versions[0].version }}</span>
      </router-link>
    </a-menu-item>
    <a-menu-item @click="toggleCollapsed" key="challenge">
      <template #icon>
        <unicon name="brackets-curly"></unicon>
      </template>
      <router-link to="/challenge">
        Thử thách
      </router-link>
    </a-menu-item>
   </a-menu> 
</template>
