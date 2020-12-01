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
      innerRadius: 20,
      outerRadius: 85,
      margin: {
        top: 50,
        right: 75,
        bottom: 50,
        left: 75,
      },
      groupLabelMargin: 20,
      valueLabelMargin: 1,
      fontSize: 10,
    };
  },
  computed: {
    data: function () {
      return [
        {
          label: "研究に対する<br/>態度",
          children: [
            {
              label: "個人",
              value: this.values["研究に対する態度"],
              color: "#A2DEFA",
            },
          ],
        },
        {
          label: "ネットワーク・<br/>異分野融合",
          children: [
            {
              label: "個人",
              value: this.values["ネットワーク・異分野融合"],
              color: "#B8F793",
            },
          ],
        },
        {
          label: "経験への<br/>開かれ",
          children: [
            {
              label: "個人",
              value: this.values["経験への開かれ"],
              color: "#FF9C9F",
            },
          ],
        },
        {
          label: "モチベー<br/>ション",
          children: [
            {
              label: "個人",
              value: this.values["モチベーション"],
              color: "#ACF5EF",
            },
          ],
        },
        {
          label: "目的と手段の整合",
          children: [
            {
              label: "個人",
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
