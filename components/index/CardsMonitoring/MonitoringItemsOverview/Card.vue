<template>
  <v-col cols="12" md="6" class="DataCard MonitoringItemsOverviewCard">
    <client-only>
      <data-view
        :title="$t('モニタリング項目')"
        title-id="monitoring-items-overview"
        :date="monitoringItemsData.date"
      >
        <template v-if="$route.path !== localePath('/monitoring')" #description>
          <app-link
            :to="localePath('/monitoring')"
            :class="[$style.button, $style['inner-link']]"
          >
            {{ $t('モニタリング項目の各グラフはこちら') }}
          </app-link>
        </template>
        <template #additionalDescription>
          <span>{{ $t('（注）') }}</span>
          <ul>
            <li>
              <i18n
                tag="span"
                path="{number}：急病やけがの際に、緊急受診の必要性や診察可能な医療機関をアドバイスする電話相談窓口"
              >
                <template #number>
                  <dfn>#7119</dfn>
                </template>
              </i18n>
            </li>
            <li>
              {{
                $t(
                  '救急医療の東京ルールの適用件数：救急隊による5医療機関への受入要請又は選定開始から20分以上経過しても搬送先が決定しない事案の件数'
                )
              }}
            </li>
            <li>
              {{ $t('(1)～(5)は7日間移動平均で算出') }}
            </li>
            <li>
              {{ $t('(2)(4)(5)は報告日の前日時点の数値') }}
            </li>
            <li>
              {{ $t('(6)の確保病床数には、(7)の確保病床数を含む') }}
            </li>
            <li>
              {{ $t('速報値として公表するものであり、後日修正する場合がある') }}
            </li>
            <li>
              {{
                $t(
                  '(2)(5)は土曜日、日曜日、祝日は更新しない。(4)は日曜日、祝日は更新しない'
                )
              }}
            </li>
            <li>
              {{
                $t(
                  '(1)(3)には、感染者の濃厚接触者が有症状となった場合で、検査を実施せずに医師の判断により臨床診断された患者を含む'
                )
              }}
            </li>
          </ul>
        </template>
        <section :class="$style.section">
          <h4>{{ $t('感染状況') }}</h4>
          <infection-status
            :aria-label="$t('感染状況')"
            :items="monitoringItems"
          />
        </section>
        <section :class="$style.section">
          <h4>{{ $t('医療提供体制') }}</h4>
          <medical-system
            :aria-label="$t('医療提供体制')"
            :items="monitoringItems"
          />
        </section>
        <div :class="$style['button-wrap']">
          <app-link
            :class="$style.button"
            to="https://www.fukushihoken.metro.tokyo.lg.jp/iryo/kansen/corona_portal/info/monitoring.html"
          >
            {{ $t('最新のモニタリング項目の分析・総括コメントについて') }}
          </app-link>
        </div>
        <div>
          <app-link
            :class="$style.button"
            to="https://www.fukushihoken.metro.tokyo.lg.jp/iryo/kansen/corona_portal/info/kunishihyou.html"
          >
            {{ $t('国の新しいレベル分類のための指標') }}
          </app-link>
        </div>
      </data-view>
    </client-only>
  </v-col>
</template>

<script>
import AppLink from '@/components/_shared/AppLink.vue'
import DataView from '@/components/index/_shared/DataView.vue'
import InfectionStatus from '@/components/index/CardsMonitoring/MonitoringItemsOverview/Table/InfectionStatus.vue'
import MedicalSystem from '@/components/index/CardsMonitoring/MonitoringItemsOverview/Table/MedicalSystem.vue'
import monitoringItemsData from '@/data/monitoring_items.json'
import { formatMonitoringItems } from '@/utils/formatMonitoringItems'

export default {
  components: {
    DataView,
    InfectionStatus,
    MedicalSystem,
    AppLink,
  },
  data() {
    const monitoringItems = formatMonitoringItems(monitoringItemsData.data)
    return {
      monitoringItemsData,
      monitoringItems,
    }
  },
}
</script>

<style lang="scss" module>
.section {
  margin: 0 0 20px;

  /* h タグが連続するため DataView-Content の margin を少し打ち消す */
  &:first-child {
    margin-top: -10px;
  }

  h4 {
    color: $gray-2;
    margin: 5px 0 10px;
    font-weight: normal;

    @include font-size(16);
  }
}

.button {
  color: $green-1 !important;

  &:hover {
    color: $white !important;
  }

  @include button-text('sm');
}

.button-wrap {
  margin-bottom: 16px;
}

.inner-link {
  text-decoration: none;
}

dfn {
  font-style: normal;
  font-weight: 600;
}
</style>
