<template>
  <svg :viewBox="`0 0 ${width} ${height}`">
    <g :transform="`translate(${width / 2},${height / 2})`">
      <g>
        <g v-for="(group, i) in values" :key="group.label">
          <line
            :x1="innerRadius * Math.cos(groupRotate(i) + dt / 2)"
            :y1="innerRadius * Math.sin(groupRotate(i) + dt / 2)"
            :x2="outerRadius * Math.cos(groupRotate(i) + dt / 2)"
            :y2="outerRadius * Math.sin(groupRotate(i) + dt / 2)"
            :stroke="lineColor"
          />
        </g>
      </g>
      <g>
        <g v-for="v in ticks" :key="v">
          <circle cx="0" cy="0" :r="r(v)" fill="none" :stroke="lineColor" />
        </g>
      </g>
      <g>
        <g v-for="(group, i) in values" :key="group.label">
          <foreignObject
            :x="
              (outerRadius + groupLabelMargin) * Math.cos(groupRotate(i)) +
              foreignObjectOffsetX(groupRotate(i))
            "
            :y="
              (outerRadius + groupLabelMargin) * Math.sin(groupRotate(i)) +
              foreignObjectOffsetY(groupRotate(i))
            "
            :width="foreignObjectSize + 'px'"
            :height="foreignObjectSize + 'px'"
          >
            <div
              :style="{
                justifyContent: foreignObjectJustifyContent(groupRotate(i)),
                alignItems: foreignObjectAlignItems(groupRotate(i)),
              }"
            >
              <span
                :style="{
                  color: fontColor,
                  fontSize: fontSize + 'px',
                  fontFamily,
                }"
                v-html="group.label"
              ></span>
            </div>
          </foreignObject>
        </g>
      </g>
      <g>
        <g v-for="(group, i) in values" :key="group.label">
          <g v-for="(item, j) in group.children" :key="item.label">
            <path
              :d="contentPath(item.value, i, j)"
              :fill="item.color"
              :opacity="barOpacity"
            />
            <text
              :x="(r(item.value) + valueLabelMargin) * Math.cos(groupRotate(i))"
              :y="(r(item.value) + valueLabelMargin) * Math.sin(groupRotate(i))"
              :text-anchor="axisTextAnchor(groupRotate(i))"
              :dominant-baseline="axisDominantBaseline(groupRotate(i))"
              :fill="fontColor"
              :font-size="fontSize"
              :font-family="fontFamily"
              font-weight="bold"
            >
              {{ item.value.toFixed(0) }}%
            </text>
          </g>
        </g>
      </g>
    </g>
  </svg>
</template>
<script>
export default {
  data() {
    return {
      barOpacity: 1,
      barAngle: 0.35,
      barMarginAngle: 0.05,
      lineColor: "#EDEDF0",
      fontColor: "#5D5D5D",
      fontFamily: `"游ゴシック", "Yu Gothic", "游ゴシック体", YuGothic, sans-serif`,
      ticks: [0, 20, 40, 60, 80, 100],
      foreignObjectSize: 200,
    };
  },
  props: [
    "innerRadius",
    "outerRadius",
    "margin",
    "groupLabelMargin",
    "valueLabelMargin",
    "fontSize",
    "values",
  ],
  computed: {
    width: function () {
      return this.margin.left + this.margin.right + this.outerRadius * 2;
    },
    height: function () {
      return this.margin.top + this.margin.bottom + this.outerRadius * 2;
    },
    dt: function () {
      return (Math.PI * 2) / this.values.length;
    },
  },
  methods: {
    groupRotate: function (i) {
      const t0 = -Math.PI / 2;
      return t0 + this.dt * i + this.dt;
    },
    itemRotate: function (i, j) {
      const a = this.barAngle + this.barMarginAngle;
      const contentT = a * (this.values[i].children.length - 1) + this.barAngle;
      return this.groupRotate(i) - contentT / 2 + a * j;
    },
    r: function (v) {
      return (
        ((this.outerRadius - this.innerRadius) * v) /
          this.ticks[this.ticks.length - 1] +
        this.innerRadius
      );
    },
    contentPath: function (v, i, j) {
      const t = this.itemRotate(i, j);
      const r0 = this.innerRadius;
      const r1 = this.r(v);
      const x1 = r0 * Math.cos(t);
      const y1 = r0 * Math.sin(t);
      const x2 = r0 * Math.cos(t + this.barAngle);
      const y2 = r0 * Math.sin(t + this.barAngle);
      const x3 = r1 * Math.cos(t);
      const y3 = r1 * Math.sin(t);
      const x4 = r1 * Math.cos(t + this.barAngle);
      const y4 = r1 * Math.sin(t + this.barAngle);
      return `M ${x3},${y3} A ${r1} ${r1} 0 0 1 ${x4},${y4} L ${x2},${y2} A ${r0} ${r0} 0 0 0 ${x1},${y1} Z`;
    },
    axisTextAnchor: function (t) {
      const cosT = Math.cos(t);
      if (-0.01 <= cosT && cosT <= 0.01) {
        return "middle";
      }
      return cosT > 0 ? "start" : "end";
    },
    axisDominantBaseline: function (t) {
      const sinT = Math.sin(t);
      if (-0.01 <= sinT && sinT <= 0.01) {
        return "central";
      }
      return sinT > 0 ? "text-before-edge" : "text-after-edge";
    },
    foreignObjectOffsetX: function (t) {
      const h = this.axisTextAnchor(t);
      if (h === "start") {
        return 0;
      } else if (h === "end") {
        return -this.foreignObjectSize;
      } else {
        return -this.foreignObjectSize / 2;
      }
    },
    foreignObjectOffsetY: function (t) {
      const v = this.axisDominantBaseline(t);
      if (v === "text-after-edge") {
        return -this.foreignObjectSize;
      } else if (v === "text-before-edge") {
        return 0;
      } else {
        return -this.foreignObjectSize / 2;
      }
    },
    foreignObjectJustifyContent: function (t) {
      const h = this.axisTextAnchor(t);
      if (h === "start") {
        return "flex-start";
      } else if (h === "end") {
        return "flex-end";
      } else {
        return "center";
      }
    },
    foreignObjectAlignItems: function (t) {
      const v = this.axisDominantBaseline(t);
      if (v === "text-after-edge") {
        return "flex-end";
      } else if (v === "text-before-edge") {
        return "flex-start";
      } else {
        return "center";
      }
    },
  },
};
</script>
<style scoped>
foreignObject > div > span {
  font-weight: bold;
  margin: 0;
  text-align: center;
  line-height: 1.6;
}
foreignObject > div {
  display: flex;
  height: 100%;
}
text {
  transform-origin: 0px 0px;
}
</style>
