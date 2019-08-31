<template>
  <div id="app">
    <el-row>
      <el-col :span="24">
        <div class="info">
          建造格子：{{trade+manufacture+powerPlant}}/9 总电力：{{powerTR}}/{{powerT}}
          控制中枢
          <!--{{CClevel}}-->
          5级
          <el-button type="text" class="el-icon-minus" v-on:click="SReduce(9)"></el-button>
          <el-button type="text" class="el-icon-plus" v-on:click="SAdd(9)"></el-button>
        </div>
      </el-col>
    </el-row>
    <el-row style="margin-right:1px">
      <el-col :xs="24" :sm="8" :md="8" :lg="6" :xl="6">
        <div class="title">
          贸易站
          <span style="margin-right:50px">{{trade}}/5</span>
          <el-button type="text" class="el-icon-minus" v-on:click="SReduce(1)"></el-button>
          <el-button type="text" class="el-icon-plus" v-on:click="SAdd(1)"></el-button>
        </div>
        <div style="margin:0 auto;width:100%">
          <div v-for="(item,index) in TCounter" :key="'T'+index" style="margin:1 auto">
            <trade :number="index+1" :powerR="powerT-powerTR" @updateT="getTPower"></trade>
          </div>
        </div>
      </el-col>
      <el-col :xs="24" :sm="8" :md="8" :lg="6" :xl="6">
        <div class="title">
          制造站
          <span style="margin-right:50px">{{manufacture}}/5</span>
          <el-button type="text" class="el-icon-minus" v-on:click="SReduce(2)"></el-button>
          <el-button type="text" class="el-icon-plus" v-on:click="SAdd(2)"></el-button>
        </div>
        <div style="margin:0 auto;width:100%">
          <div v-for="(item,index) in MCounter" :key="'M'+index" style="margin:1 auto">
            <manufacture :number="index+1" :powerR="powerT-powerTR" @updateM="getMPower"></manufacture>
          </div>
        </div>
      </el-col>
      <el-col :xs="24" :sm="8" :md="8" :lg="6" :xl="6">
        <div class="title">
          发电站
          <span style="margin-right:50px">{{powerPlant}}/3</span>
          <el-button type="text" class="el-icon-minus" v-on:click="SReduce(3)"></el-button>
          <el-button type="text" class="el-icon-plus" v-on:click="SAdd(3)"></el-button>
        </div>
        <div v-for="(item,index) in PCounter" :key="'P'+index" style="float:left">
          <power-plant :number="index+1" :powerR="powerT-powerTR" @update="getPower"></power-plant>
        </div>
      </el-col>
      <el-col :xs="24" :sm="8" :md="8" :lg="6" :xl="6">
        <div class="title">
          宿舍
          <span style="margin-right:50px">{{dormitory}}/4</span>
          <el-button type="text" class="el-icon-minus" v-on:click="SReduce(4)"></el-button>
          <el-button type="text" class="el-icon-plus" v-on:click="SAdd(4)"></el-button>
        </div>
        <div v-for="(item,index) in DCounter" :key="'D'+index" style="float:left">
          <dormitory :number="index+1" :powerR="powerT-powerTR" @update="getDPower"></dormitory>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import powerPlant from "./components/powerPlant.vue";
import manufacture from "./components/manufacture.vue";
import trade from "./components/trade.vue";
import dormitory from "./components/dormitory.vue";
import employee from "@/assets/employee.json";

export default {
  name: "app",
  data: function() {
    return {
      trade: 0,
      manufacture: 0,
      powerPlant: 0,
      dormitory: 0,
      meetingRoom: 0,
      machiningRoom: 0,
      humanOffice: 0,
      trainingRoom: 0,
      Dpower: [0, 0, 0, 0],
      Tpower: [0, 0, 0, 0, 0],
      Mpower: [0, 0, 0, 0, 0],
      power: [0, 0, 0],
      powerT: 0,
      powerTR: 0, //电力使用
      TCounter: [],
      MCounter: [],
      PCounter: [],
      DCounter: []
    };
  },
  components: { powerPlant, manufacture, trade, dormitory },
  methods: {
    updatePower: function() {
      // eslint-disable-next-line
      console.log(employee);
      this.powerT = this.power[0] + this.power[1] + this.power[2];
      this.powerTR =
        this.Tpower[0] +
        this.Tpower[1] +
        this.Tpower[2] +
        this.Tpower[3] +
        this.Tpower[4] +
        this.Mpower[0] +
        this.Mpower[1] +
        this.Mpower[2] +
        this.Mpower[3] +
        this.Mpower[4] +
        this.Dpower[0] +
        this.Dpower[1] +
        this.Dpower[2] +
        this.Dpower[3];
    },
    SAdd: function(x) {
      if (x === 4) {
        if (this.powerT - this.powerTR >= 10) {
          if (this.dormitory < 4) {
            this.dormitory += 1;
            this.DCounter.push({ id: this.DCounter.dormitory });
            this.Dpower[this.dormitory - 1] = 10;
          }
        }
      } else if (this.trade + this.manufacture + this.powerPlant < 9) {
        switch (x) {
          case 1:
            if (this.powerT - this.powerTR >= 10) {
              if (this.trade < 5) {
                this.trade += 1;
                this.TCounter.push({ id: this.TCounter.trade });
                this.Tpower[this.trade - 1] = 10;
              }
            }
            break;
          case 2:
            if (this.powerT - this.powerTR >= 10) {
              if (this.manufacture < 5) {
                this.manufacture += 1;
                this.MCounter.push({ id: this.MCounter.manufacture });
                this.Mpower[this.manufacture - 1] = 10;
              }
            }
            break;
          case 3:
            if (this.powerPlant < 3) {
              this.powerPlant += 1;
              this.PCounter.push({ id: this.PCounter.powerPlant });
              this.power[this.powerPlant - 1] = 60;
            }
            break;
        }
      }
      this.updatePower();
      this.$forceUpdate();
    },
    SReduce: function(x) {
      var i;
      switch (x) {
        case 1:
          if (this.trade > 0) {
            this.trade -= 1;
            i = this.TCounter.indexOf(this.TCounter);
            this.TCounter.splice(i, 1);
            this.Tpower[this.trade] = 0;
          }
          break;
        case 2:
          if (this.manufacture > 0) {
            this.manufacture -= 1;
            i = this.MCounter.indexOf(this.MCounter);
            this.MCounter.splice(i, 1);
            this.Mpower[this.manufacture] = 0;
          }
          break;
        case 3:
          if (
            this.powerPlant > 0 &&
            this.powerT - this.powerTR - this.power[this.powerPlant - 1] >= 0
          ) {
            this.powerPlant -= 1;
            i = this.PCounter.indexOf(this.PCounter);
            this.PCounter.splice(i, 1);
            this.power[this.powerPlant] = 0;
          }
          break;
        case 4:
          if (this.dormitory > 0) {
            this.dormitory -= 1;
            i = this.DCounter.indexOf(this.DCounter);
            this.DCounter.splice(i, 1);
            this.Dpower[this.dormitory] = 0;
          }
          break;
      }
      this.updatePower();
      this.$forceUpdate();
    },
    getPower: function(power, number) {
      this.power[number - 1] = power;
      this.updatePower();
      this.$forceUpdate();
    },
    getTPower: function(power, number) {
      this.Tpower[number - 1] = power;
      this.updatePower();
      this.$forceUpdate();
    },
    getMPower: function(power, number) {
      this.Mpower[number - 1] = power;
      this.updatePower();
      this.$forceUpdate();
    },
    getDPower: function(power, number) {
      this.Dpower[number - 1] = power;
      this.updatePower();
      this.$forceUpdate();
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
.title {
  border: 1px solid black;
  padding: 5px;
  padding-left: 25px;
  margin-bottom: 3px;
  margin: 1px auto;
  height: 40px;
  line-height: 40px;
}
.row {
  height: 180px;
  width: 300px;
  float: left;
}
.info {
  width: 100%;
  text-align: center;
  height: 50px;
  line-height: 50px;
  border: 1px solid black;
  margin-bottom: 3px;
}
</style>
