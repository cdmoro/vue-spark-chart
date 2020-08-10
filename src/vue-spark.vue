<template>
  <div class="vue-spark">
    <svg :width="width" :height="height">
      <polyline
        v-if="fill"
        stroke-linejoin="round"
        stroke-linecap="round"
        stroke="none"
        :stroke-width="strokeWidth"
        :fill="fill"
        :points="pointsCoordsFill"
      ></polyline>
      <polyline
        v-if="!noStroke"
        stroke-linejoin="round"
        stroke-linecap="round"
        :stroke="strokeColor"
        :stroke-width="strokeWidth"
        fill="none"
      >
        <animate
          attributeName="points"
          dur="0.35s"
          repeatCount="1"
          fill="freeze"
          :from="pointsCoordsFrom"
          :to="pointsCoords"
        ></animate>
      </polyline>
      <polyline v-if="avg" stroke-width="1" :stroke="avgColor" stroke-dasharray="4 3">
        <animate
          begin="0.4s"
          attributeName="points"
          dur="0.3s"
          repeatCount="1"
          fill="freeze"
          :from="avgCoordsFrom"
          :to="avgCoords"
        ></animate>
      </polyline>
    </svg>
  </div>
</template>

<script>
export default {
  name: "Spark",
  props: {
    avg: { type: Boolean, default: false },
    avgColor: { type: String, default: "#999" },
    height: { type: String, default: "30" },
    fill: { type: String, default: null },
    noStroke: { type: Boolean, default: false },
    points: { type: Array, default: () => [] },
    size: { type: String, default: "md" },
    strokeColor: { type: String, default: "#333" },
    strokeWidth: { type: String, default: "2" },
    width: { type: String, default: "90" }
  },
  data() {
    return {
      min: 0
    };
  },
  computed: {
    average() {
      const sum = this.points.reduce(function(a, b) {
        return a + b;
      });

      return Math.floor(
        (Math.floor(sum / this.points.length) * this.height) / this.maxPoint
      );
    },
    avgCoords() {
      const avg = this.height - this.average;
      return `0,${avg} ${this.width},${avg}`;
    },
    avgCoordsFrom() {
      const avg = this.height - this.average;
      return `0,${avg} 0,${avg}`;
    },
    maxPoint() {
      return this.points.reduce(function(a, b) {
        return Math.max(a, b);
      });
    },
    stepX() {
      return Math.floor(this.width / (this.points.length - 1));
    },
    pointsCoordsFrom() {
      let points = [];
      for (let i = 0; i < this.points.length; i++) {
        const x = i * this.stepX;
        let offsetX = parseInt(
          i === 0
            ? this.strokeWidth
            : i === this.points.length
            ? -1 * this.strokeWidth
            : 0
        );

        points.push(`${x + offsetX},${this.height - this.strokeWidth}`);
      }
      return points.join(" ");
    },
    pointsCoords() {
      let points = [];
      for (let i = 0; i < this.points.length; i++) {
        const x = i * this.stepX;
        let offsetX = parseInt(
          i === 0
            ? this.strokeWidth
            : i === this.points.length
            ? -1 * this.strokeWidth
            : 0
        );

        const y = this.height - Math.floor(
          (this.points[i] * (this.height - this.strokeWidth)) / this.maxPoint
        );

        const offsetY = parseInt(
          this.points[i] === this.min
            ? -1 * this.strokeWidth
            : this.points[i] === this.max
            ? this.strokeWidth
            : 0
        );

        points.push(`${x + offsetX},${y + offsetY}`);
      }
      return points.join(" ");
    },
    pointsCoordsFill() {
      let points = [];
      for (let i = 0; i < this.points.length; i++) {
        let x = i * this.stepX;
        let offsetX = parseInt(
          i === 0
            ? this.strokeWidth
            : i === this.points.length
            ? -1 * this.strokeWidth
            : 0
        );

        const y = this.height - Math.floor(
          (this.points[i] * (this.height - this.strokeWidth)) / this.maxPoint
        );

        const offsetY = parseInt(
          this.points[i] === this.min
            ? -1 * this.strokeWidth
            : this.points[i] === this.max
            ? this.strokeWidth
            : 0
        );

        if (i === 0) points.push(`${Math.floor(offsetX / 2)},${this.height}`);

        else if (i < this.points.length - 1)
          points.push(`${x + Math.floor(offsetX / 2)},${y + offsetY}`);

        else if (i === this.points.length - 1) {
          points.push(`${this.width - this.strokeWidth / 2},${y + offsetY}`);
          points.push(`${this.width - this.strokeWidth / 2},${this.height}`);
        }
      }
      return points.join(" ");
    }
  }
};
</script>
