<template>
  <div class="power">
    {{number}}# 发电站
    <span style="margin-left:15px;margin-right:25px">{{level}}级</span>
    <el-button type="text" class="el-icon-minus" @click="LReduce()"></el-button>
    <el-button type="text" class="el-icon-plus" @click="LAdd()"></el-button>
    <div>产电:{{powerC}}</div>
    <div>充能速度:{{chargeSpeed}}</div>
    <div>心情消耗/时:{{moodUse}}</div>
  </div>
</template>

<script>
export default {
  name: "power",
  data: function() {
    return {
      key: 0,
      level: 1,
      powerC: 60,
      chargeSpeed: 1.00,
      moodUse: 1.0
    };
  },
  props: ["number", "powerR"],
  methods: {
    LReduce: function() {
      if (this.level > 1) {
        if (this.level === 2 && this.powerR - (130 - 60) >= 0) {
          this.level -= 1;
          this.powerC = 60;
        } else if (this.level === 3 && this.powerR - (270 - 130) >= 0) {
          this.level -= 1;
          this.powerC = 130;
        }
      }
    },
    LAdd: function() {
      if (this.level < 3) {
        this.level += 1;
        if (this.level === 2) this.powerC = 130;
        else if (this.level === 3) this.powerC = 270;
      }
    }
  },
  watch: {
    powerC: function() {
      this.$emit("update", this.powerC, this.number);
    }
  }
};
</script>

<style scoped>
.power {
  background-color: aquamarine;
  height: 100px;
  width: 200px;
  border: 1px solid black;
  padding: 5px 10px;
  margin-bottom: 3px;
}
</style>
