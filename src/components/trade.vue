<template>
  <el-col :xs="12" :sm="24" :md="24" :lg="24" :xl="12">
    <div class="trade">
      <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
          <el-col :xs="3" :sm="3" :md="3" :lg="3" :xl="3">
            <div style="height:20px;line-height:20px;margin-top:5px;">
              <span style="padding-left:0">{{number}}#</span>
              <el-button
                type="text"
                class="el-icon-remove-outline"
                style="width:100%;padding:5px 0"
                @click="LReduce()"
              ></el-button>
              <el-button
                type="text"
                class="el-icon-circle-plus-outline"
                style="margin-left:0;width:100%;padding:5px 0"
                @click="LAdd()"
              ></el-button>
            </div>
          </el-col>
          <el-col :xs="20" :sm="20" :md="20" :lg="20" :xl="20">
            <div style="font-size:12px;margin-top:6px;margin-left:15px;width:100%">
              <div>等级:{{level}}</div>
              <div>耗电:{{powerU}}</div>
              <div>生产力:{{effectiveness}}</div>
              <div>修正MU/h:{{moodUse}}</div>
            </div>
          </el-col>
      </el-col>
      <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">111</el-col>
    </div>
  </el-col>
</template>

<script>
export default {
  name: "trade",
  data: function() {
    return {
      key: 0,
      level: 1,
      powerU: 10,
      effectiveness:1.01,
      moodUse:1.0
    };
  },
  props: ["number", "powerR"],
  methods: {
    LReduce: function() {
      if (this.level > 1) {
        this.level -= 1;
        if (this.level === 1) {
          this.powerU = 10;
          this.effectiveness = 1.01;
          this.moodUse = 1;
        } else if (this.level === 2) {
          this.powerU = 30;
          this.effectiveness = 1.02;
          this.moodUse = 0.95;
        }
      }
    },
    LAdd: function() {
      if (this.level < 3) {
        if (this.level === 1 && this.powerR >= 30 - 10) {
          this.level += 1;
          this.powerU = 30;
          this.effectiveness = 1.02;
          this.moodUse = 0.95;
        } else if (this.level === 2 && this.powerR >= 60 - 30) {
          this.level += 1;
          this.powerU = 60;
          this.effectiveness = 1.03;
          this.moodUse = 0.9;
        }
      }
    }
  },
  watch: {
    powerU: function() {
      this.$emit("updateT", this.powerU, this.number);
    }
  }
};
</script>

<style scoped>
.trade {
  background-color: skyblue;
  height: 100px;
  width: 100%;
  border: 1px solid black;
  margin-bottom: 3px;
  margin: 0 auto;
}
</style>