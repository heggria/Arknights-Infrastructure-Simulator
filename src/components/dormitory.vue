<template>
  <div class="dormitory">
    {{number}}# 宿舍
    <span style="margin-left:15px;margin-right:25px">{{level}}级</span>
    <el-button type="text" class="el-icon-minus" @click="LReduce()"></el-button>
    <el-button type="text" class="el-icon-plus" @click="LAdd()"></el-button>
    <div>耗电:{{powerU}}</div>
    <div>氛围:{{atmosphereLimit}}</div>
    <div>心情恢复/时:{{moodB+moodA}} ({{moodB}}+{{moodA}})</div>
  </div>
</template>

<script>
export default {
  name: "dormitory",
  data: function() {
    return {
      key: 0,
      level: 1,
      powerU: 10,
      atmosphereLimit: 1000,
      atmosphereReal: 5000,
      moodB: 1.6,
      moodA: 0.4,
    };
  },
  props: ["number", "powerR"],
  methods: {
    LReduce: function() {
      if (this.level > 1) {
        this.level -= 1;
        if (this.level === 1) {
          this.powerU = 10;
          this.atmosphereLimit = 1000;
          this.moodB = 1.6;
        } else if (this.level === 2) {
          this.powerU = 20;
          this.atmosphereLimit = 2000;
          this.moodB = 1.7;
        } else if (this.level === 3) {
          this.powerU = 30;
          this.atmosphereLimit = 3000;
          this.moodB = 1.8;
        } else if (this.level === 4) {
          this.powerU = 45;
          this.atmosphereLimit = 4000;
          this.moodB = 1.9;
        }
      }
      this.moodA=this.atmosphereLimit>this.atmosphereReal?this.atmosphereReal/2500:this.atmosphereLimit/2500
    },
    LAdd: function() {
      if (this.level < 5) {
        if (this.level === 1 && this.powerR >= 20 - 10) {
          this.level += 1;
          this.powerU = 20;
          this.atmosphereLimit = 2000;
          this.moodB = 1.7;
        } else if (this.level === 2 && this.powerR >= 30 - 20) {
          this.level += 1;
          this.powerU = 30;
          this.atmosphereLimit = 3000;
          this.moodB = 1.8;
        } else if (this.level === 3 && this.powerR >= 45 - 30) {
          this.level += 1;
          this.powerU = 45;
          this.atmosphereLimit = 4000;
          this.moodB = 1.9;
        } else if (this.level === 4 && this.powerR >= 60 - 45) {
          this.level += 1;
          this.powerU = 60;
          this.atmosphereLimit = 5000;
          this.moodB = 2.0;
        }
      }
      this.moodA=this.atmosphereLimit>this.atmosphereReal?this.atmosphereReal/2500:this.atmosphereLimit/2500
    }
  },
  watch: {
    powerU: function() {
      this.$emit("update", this.powerU, this.number);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.dormitory {
  background-color: rgb(241, 239, 235);
  height: 100px;
  width: 200px;
  border: 1px solid black;
  padding: 5px 10px;
  margin-bottom: 3px;
}
</style>