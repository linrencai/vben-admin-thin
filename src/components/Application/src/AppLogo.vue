<!--
 * @Author: Vben
 * @Description: logo component
-->
<template>
  <div class="anticon" :class="getAppLogoClass" @click="goHome">
    <img src="../../../assets/images/logo.png" />
    <div class="ml-2 truncate md:opacity-100" :class="getTitleClass" v-show="showTitle">
      {{ title }}
    </div>
  </div>
</template>
<script lang="ts">
  import { defineComponent, computed, unref } from 'vue';
  import { useGlobSetting } from '/@/hooks/setting';
  import { useGo } from '/@/hooks/web/usePage';
  import { useMenuSetting } from '/@/hooks/setting/useMenuSetting';
  import { useDesign } from '/@/hooks/web/useDesign';
  import { PageEnum } from '/@/enums/pageEnum';

  const props = {
    /**
     * The theme of the current parent component
     */
    theme: { type: String, validator: (v) => ['light', 'dark'].includes(v) },
    /**
     * Whether to show title
     */
    showTitle: { type: Boolean, default: true },
    /**
     * The title is also displayed when the menu is collapsed
     */
    alwaysShowTitle: { type: Boolean },
  };

  export default defineComponent({
    name: 'AppLogo',
    props: props,
    setup(props) {
      const { prefixCls } = useDesign('app-logo');
      const { getCollapsedShowTitle } = useMenuSetting();
      const { title } = useGlobSetting();
      const go = useGo();

      const getAppLogoClass = computed(() => [
        prefixCls,
        props.theme,
        { 'collapsed-show-title': unref(getCollapsedShowTitle) },
      ]);

      const getTitleClass = computed(() => [
        `${prefixCls}__title`,
        {
          'xs:opacity-0': !props.alwaysShowTitle,
        },
      ]);

      function goHome() {
        go(PageEnum.BASE_HOME);
      }

      return {
        getAppLogoClass,
        getTitleClass,
        getCollapsedShowTitle,
        goHome,
        title,
        prefixCls,
      };
    },
  });
</script>
<style lang="less" scoped>
  @prefix-cls: ~'@{namespace}-app-logo';

  .@{prefix-cls} {
    display: flex;
    align-items: center;
    padding-left: 7px;
    cursor: pointer;
    transition: all 0.2s ease;

    &.light {
      border-bottom: 1px solid @border-color-base;
    }

    &.collapsed-show-title {
      padding-left: 20px;
    }

    &.light &__title {
      color: @primary-color;
    }

    &.dark &__title {
      color: @white;
    }

    &__title {
      font-size: 16px;
      font-weight: 700;
      transition: all 0.5s;
      color: #1296db !important;
    }
  }
</style>
