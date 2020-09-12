<template>
  <div>
    <rose-chart
      :innerRadius="innerRadius"
      :outerRadius="outerRadius"
      :margin="margin"
      :titleMargin="titleMargin"
      :barOpacity="barOpacity"
      :barAngle="barAngle"
      :barMarginAngle="barMarginAngle"
      :lineColor="lineColor"
      :fontColor="fontColor"
      :fontFamily="fontFamily"
      :tickFontSize="tickFontSize"
      :groupFontSize="groupFontSize"
      :itemFontSize="itemFontSize"
      :titleFontSize="titleFontSize"
      :title="title"
      :values="values"
      :ticks="ticks"
    ></rose-chart>
  </div>
</template>
<script>
import RoseChart from "../components/rose-chart.vue";

const values = [];

export default {
  data() {
    return {
      innerRadius: 80,
      outerRadius: 250,
      margin: {
        top: 20,
        right: 150,
        bottom: 20,
        left: 150,
      },
      titleMargin: 6,
      barOpacity: "0.8",
      barAngle: 0.28,
      barMarginAngle: 0.05,
      lineColor: "#ccc",
      fontColor: "#555",
      fontFamily: "sans-serif",
      tickFontSize: 10,
      groupFontSize: 10,
      itemFontSize: 8,
      titleFontSize: 14,
      title: ["多様性を", "イノベーションに", "繋ぐ評価"],
      values: [
        {
          label: "研究に対する態度",
          children: [
            { label: "個人", value: 87, color: "#a6cee3" },
            { label: "環境", value: 81, color: "#1f78b4" },
          ],
        },
        {
          label: "ネットワーク・異分野融合",
          children: [
            { label: "個人", value: 47, color: "#b2df8a" },
            { label: "環境", value: 21, color: "#33a02c" },
          ],
        },
        {
          label: "経験への開かれ",
          children: [
            { label: "個人", value: 57, color: "#fb9a99" },
            { label: "環境", value: 33, color: "#e31a1c" },
          ],
        },
        {
          label: "モチベーション",
          children: [{ label: "環境", value: 67, color: "#6a3d9a" }],
        },
        {
          label: "心理的安全性",
          children: [{ label: "環境", value: 100, color: "#ffff99" }],
        },
        {
          label: "リーダーシップ「目利き」",
          children: [{ label: "環境", value: 63, color: "#b15928" }],
        },
        {
          label: "目的と手段の整合",
          children: [
            { label: "個人", value: 100, color: "#fdbf6f" },
            { label: "環境", value: 100, color: "#ff7f00" },
          ],
        },
      ],
      ticks: [0, 20, 40, 60, 80, 100],
    };
  },
  components: {
    RoseChart,
  },
};
</script>
<style scoped>
div {
  background-color: #f7f9fc;
}
</style>
