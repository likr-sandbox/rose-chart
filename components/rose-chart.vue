<template>
  <svg :viewBox="`0 0 ${width} ${height}`">
    <g :transform="`translate(${width / 2},${height / 2})`">
      <g>
        <text
          text-anchor="middle"
          dominant-baseline="central"
          :fill="fontColor"
          :font-size="titleFontSize"
          :font-family="fontFamily"
          font-weight="bold"
        >
          <tspan
            v-for="(text, i) in title"
            :key="i"
            x="0"
            :y="titleRow * i + (-titleRow * (title.length - 1)) / 2"
          >
            {{ text }}
          </tspan>
        </text>
      </g>
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
        <g v-for="(group, i) in values" :key="group.label">
          <text
            :x="(outerRadius + 3) * Math.cos(groupRotate(i))"
            :y="(outerRadius + 3) * Math.sin(groupRotate(i))"
            :text-anchor="axisTextAnchor(groupRotate(i))"
            :dominant-baseline="axisDominantBaseline(groupRotate(i))"
            :fill="fontColor"
            :font-size="groupFontSize"
            :font-family="fontFamily"
            font-weight="bold"
          >
            {{ group.label }}
          </text>
        </g>
      </g>
      <g>
        <g v-for="v in ticks" :key="v">
          <circle cx="0" cy="0" :r="r(v)" fill="none" :stroke="lineColor" />
          <text
            x="0"
            :y="-r(v)"
            text-anchor="middle"
            dominant-baseline="central"
            :fill="fontColor"
            :font-size="tickFontSize"
            :font-family="fontFamily"
          >
            {{ v }}
          </text>
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
          </g>
        </g>
      </g>
      <g>
        <g v-for="(group, i) in values" :key="group.label">
          <g v-for="(item, j) in group.children" :key="item.label">
            <text
              :x="(innerRadius - 8) * Math.cos(itemRotate(i, j) + barAngle / 2)"
              :y="(innerRadius - 8) * Math.sin(itemRotate(i, j) + barAngle / 2)"
              text-anchor="middle"
              dominant-baseline="central"
              :fill="fontColor"
              :font-size="itemFontSize"
              :font-family="fontFamily"
            >
              {{ item.label }}
            </text>
          </g>
        </g>
      </g>
    </g>
  </svg>
</template>
<script>
export default {
  props: [
    "innerRadius",
    "outerRadius",
    "margin",
    "titleMargin",
    "barOpacity",
    "barAngle",
    "barMarginAngle",
    "lineColor",
    "fontColor",
    "fontFamily",
    "tickFontSize",
    "groupFontSize",
    "itemFontSize",
    "titleFontSize",
    "title",
    "values",
    "ticks",
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
    titleRow: function () {
      return this.titleFontSize + this.titleMargin;
    },
  },
  methods: {
    groupRotate: function (i) {
      const t0 = -Math.PI / 2;
      return t0 + this.dt * i + this.dt / 2;
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
  },
};
</script>
