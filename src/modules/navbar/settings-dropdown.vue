<template>
  <div class="wrapper">
    <div v-if="open" class="fader" @click="toggleOpen" />

    <div class="menu" :class="{ open }" @click="toggleOpen">
      <div class="item open-settings" @click="openSettings">
        <icon :icon="settingsSvg" /> <span>Settings</span>
      </div>

      <a href="https://github.com/beeequeue/yuna/issues">
        <div class="item report-bug">
          <icon :icon="bugSvg" /> <span>Report bug</span>
        </div>
      </a>

      <a href="https://github.com/beeequeue/yuna">
        <div class="item github">
          <icon :icon="githubSvg" /> <span>Source code</span>
        </div>
      </a>

      <div
        v-tooltip.left="'!! This will reload the app !!'"
        class="item clear-cache"
        @click="clearCache"
      >
        <icon :icon="clearSvg" /> <span>Clear caches</span>
      </div>

      <div class="item open-about" @click="toggleAboutModal">
        <icon :icon="infoSvg" /> <span>About</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator'
import {
  mdiBugOutline,
  mdiCached,
  mdiCogOutline,
  mdiGithub,
  mdiInformationOutline,
} from '@mdi/js'

import { EpisodeCache } from '@/lib/episode-cache'
import { toggleModal } from '@/state/app'
import Icon from '@/common/components/icon.vue'

@Component({ components: { Icon } })
export default class SettingsDropdown extends Vue {
  @Prop(Boolean) public open!: boolean
  @Prop() public toggleOpen!: () => any

  public settingsSvg = mdiCogOutline
  public bugSvg = mdiBugOutline
  public githubSvg = mdiGithub
  public clearSvg = mdiCached
  public infoSvg = mdiInformationOutline

  public clearCache() {
    EpisodeCache.clear()
    ;(this as any).$apolloProvider.defaultClient.cache.reset()

    location.reload()
  }

  public openSettings() {
    this.$router.push('/settings')
  }

  public toggleAboutModal() {
    toggleModal(this.$store, 'about')
  }
}
</script>

<style scoped lang="scss">
@import '../../colors';

.wrapper {
  position: fixed;
  top: 30px;
  left: 0;
  height: 100%;
  width: 100%;
  pointer-events: none;
}

.fader {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  pointer-events: all;
}

.menu {
  position: absolute;
  top: 50px;
  right: 0;
  min-width: 150px;
  background: $white;
  fill: $highlight;
  color: $highlight;
  font-size: 18px;
  font-weight: 600;
  pointer-events: all;
  transition: transform 0.25s;

  & a {
    color: $highlight;
    text-decoration: none;
  }

  & .item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 15px;
    cursor: pointer;

    transition: background 0.15s;

    &:hover {
      background: rgba(0, 0, 0, 0.075);
    }

    & > .icon {
      height: 1.15em;
      width: 1.15em;
      margin-right: 10px;
    }
  }

  &:not(.open) {
    transform: translateX(101%);
  }
}
</style>
