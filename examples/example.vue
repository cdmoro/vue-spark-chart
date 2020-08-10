<template>
  <div id="app" class="m-5">
    <!--spark :points="[1,9,8,12,2,0,4,6,9]" /-->
    <h4>Death simple SVG-based spark component</h4>
    <button class="btn btn-sm btn-primary" @click="spark = generateSparks()">Generate sparks!</button>
    <button class="btn btn-sm btn-primary ml-2" @click="startInterval()">Animate spark</button>
    <button class="btn btn-sm btn-danger ml-2" @click="clear()">Stop animation</button>
    <button
      class="btn btn-sm btn-primary ml-2"
      @click="avg = !avg"
    >{{ avg ? 'Hide' : 'Show' }} average</button>
    <VueSpark :points="spark" :avg="avg" width="200"/>
    <hr>
    <code>
      <div>&lt;spark :points="points"&gt;&lt;/spark&gt;</div>
      <div>let points = [1,5,8,2,2,8,4,6,1]</div>
    </code>
    <div>
      <h5>BASE</h5>
      <div
        class="d-flex py-2 align-items-center justify-content-around"
        style="border-bottom: solid 1px #CCC"
        v-for="(spark, i) in sparks"
        :key="i"
      >
        <VueSpark :points="spark"/>
        <VueSpark avg :points="spark"/>
        <VueSpark avg fill="#FFAACC" :points="spark"/>
        <VueSpark avg fill="#FFAACC" no-stroke :points="spark"/>
      </div>
    </div>
    <div class="mt-3 row">
      <div class="col">
        <h6>Base</h6>
        <VueSpark v-for="(spark, i) in sparks" :key="i" :points="spark"/>
      </div>
      <div class="col">
        <h6>With average</h6>
        <VueSpark v-for="(spark, i) in sparks" :key="i" stroke-color="#35495E" avg :points="spark"/>
      </div>
      <div class="col">
        <h6>Background</h6>
        <VueSpark
          v-for="(spark, i) in sparks"
          :key="i+'1'"
          stroke-color="#35495E"
          fill="#41B883"
          avg
          avg-color="tomato"
          :points="spark"
        />
      </div>
      <div class="col">
        <h6>Just background</h6>
        <VueSpark v-for="(spark, i) in sparks" :key="i+'2'" no-stroke fill="#41B883" :points="spark"/>
      </div>
    </div>
  </div>
</template>

<script>
import VueSpark from "@/vue-spark";

export default {
  name: "App",
  components: {
    VueSpark
  },
  mounted() {},
  data() {
    return {
      spark: this.generateSparks(),
      avg: false,
      interval: null,
      sparks: [
        [1, 5, 8, 2, 2, 8, 4, 6, 1],
        [10, 9, 8, 2, 2, 0, 4, 6, 9],
        [1, 9, 18, 12, 2, 1, 4, 7, 6],
        [4, 3, 8, 1, 2, 0, 4, 0, 9],
        [1, 9, 6, 2, 2, 2, 12, 6, 1],
        [1, 9, 5, 12, 2, 0, 4, 6, 9]
      ]
    };
  },
  methods: {
    generateSparks() {
      return [...Array(20)].map(() => Math.round(Math.random() * 10));
    },
    startInterval() {
      this.interval = setInterval(() => {
        this.spark.push(Math.round(Math.random() * 10));
        this.spark.shift();
      }, 200);
    },
    clear() {
      clearInterval(this.interval);
    }
  }
};
</script>

<style>
@import 'https://unpkg.com/bootstrap/dist/css/bootstrap.min.css';
</style>