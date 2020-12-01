<template>
  <rose-chart
    :innerRadius="innerRadius"
    :outerRadius="outerRadius"
    :margin="margin"
    :groupLabelMargin="groupLabelMargin"
    :valueLabelMargin="valueLabelMargin"
    :fontSize="fontSize"
    :values="data"
  ></rose-chart>
</template>
<script>
import RoseChart from "./rose-chart.vue";

export default {
  props: ["values"],
  data() {
    return {
      innerRadius: 35,
      outerRadius: 180,
      margin: {
        top: 50,
        right: 175,
        bottom: 50,
        left: 175,
      },
      groupLabelMargin: 30,
      valueLabelMargin: 1,
      fontSize: 12,
    };
  },
  computed: {
    data: function () {
      return [
        {
          label: "研究に対する態度",
          children: [
            {
              label: "環境",
              value: this.values["研究に対する態度"],
              color: "#A2DEFA",
            },
          ],
        },
        {
          label: "ネットワーク・<br/>異分野融合",
          children: [
            {
              label: "環境",
              value: this.values["ネットワーク・異分野融合"],
              color: "#B8F793",
            },
          ],
        },
        {
          label: "経験への開かれ",
          children: [
            {
              label: "環境",
              value: this.values["経験への開かれ"],
              color: "#FF9C9F",
            },
          ],
        },
        {
          label: "心理的安全性",
          children: [
            {
              label: "環境",
              value: this.values["心理的安全性"],
              color: "#F2F24C",
            },
          ],
        },
        {
          label: "リーダーシップ<br/>「目利き」",
          children: [
            {
              label: "環境",
              value: this.values["リーダーシップ「目利き」"],
              color: "#E6B8F8",
            },
          ],
        },
        {
          label: "目的と手段の整合",
          children: [
            {
              label: "環境",
              value: this.values["目的と手段の整合"],
              color: "#FFCF74",
            },
          ],
        },
      ];
    },
  },
  components: {
    RoseChart,
  },
};
</script>
