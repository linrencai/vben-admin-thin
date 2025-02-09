<template>
  <PageWrapper :class="prefixCls">
    <template #headerContent>
      <div class="flex items-center justify-center px-2">
        <a-input-search
          v-model:value="value"
          size="large"
          :placeholder="t('common.searchPlacehoder')"
          style="width: 680px; margin: 10px auto 24px auto"
          :maxlength="50"
          @search="onSearch"
        />
      </div>
    </template>
    <div>
      <a-list :grid="grid" :data-source="list">
        <template #renderItem="{ item }">
          <a-list-item @click="goDetail(item)">
            <a-card :hoverable="true" :class="`${prefixCls}__card`">
              <div :class="`${prefixCls}__card-img`">
                <img :src="item.src" :alt="item.model" />
              </div>
              <div :class="`${prefixCls}__card-detail`">
                <p class="font-bold lrc-text-balck">{{ item.desc }}</p>
                <p class="font-bold lrc-text-balck">{{ item.listing }}</p>
                <p class="py-2">{{ item.model }}</p>
                <p class="lrc-text-balck"
                  >{{ t('routes.dashboard.mainpage.price') }}{{ item.price }}</p
                >
              </div>
            </a-card>
          </a-list-item>
        </template>
      </a-list>
      <div
        v-show="list.length"
        :style="{ textAlign: 'center', height: '32px', lineHeight: '32px' }"
      >
        <a-button @click="loadMoreData">{{ t('common.loadMore') }}</a-button>
      </div>
    </div>
    <Loading :loading="loading" :absolute="absolute" :tip="tip" theme="dark" />
  </PageWrapper>
</template>
<script lang="ts">
  import { defineComponent, ref, reactive, toRefs } from 'vue';
  import { PageWrapper } from '/@/components/Page';
  import { Card, Row, Col, List } from 'ant-design-vue';
  import banner4 from '/@/assets/images/shose.webp';
  import skonImg from '/@/assets/images/logo.png';
  import { useI18n } from '/@/hooks/web/useI18n';
  import { Loading } from '/@/components/Loading';
  import { useGo } from '/@/hooks/web/usePage';
  interface listItem {
    src: string;
    price: number;
    desc: string;
    model: string;
    listing: string;
    id: number;
  }
  const list: listItem[] = [
    {
      src: banner4,
      price: 100,
      desc: 'Nike Air Jordan 1 Retro High OG ',
      listing: ' Basketball Shoes/Sneakers',
      model: 'Electro Orange',
      id: 1,
    },
    {
      src: banner4,
      price: 100,
      desc: 'Nike Air Jordan 1 Retro High OG ',
      listing: ' Basketball Shoes/Sneakers',
      model: 'Electro Orange',
      id: 2,
    },
    {
      src: banner4,
      price: 100,
      desc: 'Nike Air Jordan 1 Retro High OG ',
      listing: ' Basketball Shoes/Sneakers',
      model: 'Electro Orange',
      id: 3,
    },
    {
      src: banner4,
      price: 100,
      desc: 'Nike Air Jordan 1 Retro High OG ',
      listing: ' Basketball Shoes/Sneakers',
      model: 'Electro Orange',
      id: 4,
    },
    {
      src: banner4,
      price: 100,
      desc: 'Nike Air Jordan 1 Retro High OG ',
      listing: ' Basketball Shoes/Sneakers',
      model: 'Electro Orange',
      id: 5,
    },
    {
      src: banner4,
      price: 100,
      desc: 'Nike Air Jordan 1 Retro High OG ',
      listing: ' Basketball Shoes/Sneakers',
      model: 'Electro Orange',
      id: 6,
    },
    {
      src: banner4,
      price: 100,
      desc: 'Nike Air Jordan 1 Retro High OG ',
      listing: ' Basketball Shoes/Sneakers',
      model: 'Electro Orange',
      id: 7,
    },
  ];
  export default defineComponent({
    components: {
      PageWrapper,
      [Card.name]: Card,
      [List.name]: List,
      [List.Item.name]: List.Item,
      [Row.name]: Row,
      [Col.name]: Col,
      Loading,
    },
    setup() {
      const { t } = useI18n();
      const go = useGo();
      const listData: any = ref([]);
      const compState = reactive({
        absolute: false,
        loading: false,
        tip: t('common.loadingText'),
      });
      const value = ref<string>('');

      const onSearch = (searchValue: string) => {
        console.log(searchValue);
        loadMoreData();
      };
      const grid: any = { gutter: 16, xs: 2, sm: 2, md: 3, lg: 4, xl: 5, xxl: 5 };

      function loadMoreData() {
        compState.loading = true;
        list.forEach((v) => {
          listData.value.push(v);
        });
        setTimeout(() => {
          compState.loading = false;
        }, 1000);
      }
      const goDetail = (v) => {
        console.log(v);
        go('/product/detail/' + v.id);
      };
      return {
        prefixCls: 'list-card',
        list: reactive(listData),
        t,
        loadMoreData,
        grid,
        ...toRefs(compState),
        skonImg,
        onSearch,
        value,
        goDetail,
      };
    },
  });
</script>
<style lang="less" scoped>
  :deep(.ant-spin-container) {
    padding: 0 24px;
  }

  .list-card {
    &__card {
      width: 100%;
      margin-bottom: -8px;
      border: none !important;

      :deep(.ant-card-body) {
        padding: 16px;
      }

      &-img {
        // width: 320px;
        // height: 200px;
        margin: 0 auto;
        font-size: 16px;
        font-weight: 500;
        color: @text-color;
        overflow: hidden;

        img {
          // width: 100%;
          object-fit: cover;
          object-position: center;
        }
      }

      &-detail {
        padding-top: 10px;
        font-size: 14px;
        text-align: center;
        color: @text-color-secondary;

        p {
          margin-bottom: 0;
          line-height: 1;
        }

        .lrc-text-balck {
          color: #000;
        }
      }
    }

    &__skeleton {
      display: flex;
      align-content: center;
      justify-content: center;
    }
  }
</style>
