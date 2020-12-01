<template>
  <div class="container">
    <div class="pc">
      <h2>PC</h2>
      <h3>個人</h3>
      <div>
        <rose-chart-personal-pc
          :values="values.personal"
        ></rose-chart-personal-pc>
      </div>
      <h3>環境</h3>
      <div class="chart-wrapper">
        <rose-chart-environmental-pc
          :values="values.environmental"
        ></rose-chart-environmental-pc>
      </div>
    </div>
    <div class="sp">
      <h2>SP</h2>
      <h3>個人</h3>
      <div>
        <rose-chart-personal-sp
          :values="values.personal"
        ></rose-chart-personal-sp>
      </div>
      <h3>環境</h3>
      <div class="chart-wrapper">
        <rose-chart-environmental-sp
          :values="values.environmental"
        ></rose-chart-environmental-sp>
      </div>
    </div>
  </div>
</template>
<script>
import RoseChartEnvironmentalPc from "../components/rose-chart-environmental-pc.vue";
import RoseChartPersonalPc from "../components/rose-chart-personal-pc.vue";
import RoseChartEnvironmentalSp from "../components/rose-chart-environmental-sp.vue";
import RoseChartPersonalSp from "../components/rose-chart-personal-sp.vue";

export default {
  data() {
    return {
      values: {
        personal: {
          研究に対する態度: 100,
          "ネットワーク・異分野融合": 100,
          経験への開かれ: 100,
          モチベーション: 100,
          目的と手段の整合: 100,
        },
        environmental: {
          研究に対する態度: 100,
          "ネットワーク・異分野融合": 100,
          経験への開かれ: 100,
          心理的安全性: 100,
          "リーダーシップ「目利き」": 100,
          目的と手段の整合: 100,
        },
      },
    };
  },
  components: {
    RoseChartEnvironmentalPc,
    RoseChartPersonalPc,
    RoseChartEnvironmentalSp,
    RoseChartPersonalSp,
  },
};
</script>
<style scoped>
.pc {
  width: 740px;
}

.sp {
  width: 320px;
}
</style>
