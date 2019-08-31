<template>
  <el-col :xs="12" :sm="24" :md="24" :lg="24" :xl="12">
    <div class="manufacture">
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
              <div>制造时间:{{value!==''?(parseInt(choose[value-1].time*num/60)+':'+(choose[value-1].time*num%60===0?'00':choose[value-1].time*num%60)):'未知'}}</div>
            </div>
          </el-col>
      </el-col>
      <el-col :xs="12" :sm="12" :md="12" :lg="12" :xl="12">
        <div style="float:left;width:100%;">
          <div style="float:left;width:100%;margin-top:5px;">
            <el-select
              size="mini"
              slot="prepend"
              v-model="value"
              placeholder="制造物"
              style="width:100%"
            >
              <el-option-group v-for="group in options1" :key="group.label">
                <el-option
                  v-for="item in group.options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                ></el-option>
              </el-option-group>
            </el-select>
            <div style="float:left;width:100%">
              <el-input-number
                size="mini"
                style="width:100%"
                controls-position="right"
                v-model="num"
                :min="0"
                :max="100"
              ></el-input-number>
            </div>
          </div>
          <div style="float:left;width:100%">
            <el-select size="mini" style="width:100%" v-model="value1" multiple placeholder="请选择">
              <el-option
                v-for="item in options2"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </div>
        </div>
      </el-col>
    </div>
  </el-col>
</template>

<script>
import employee from "@/assets/employee.json";
export default {
  name: "manufacture",
  data: function() {
    return {
      prop: { multiple: true },
      options1: [
        {
          options: [
            {
              value: "1",
              label: "基础作战记录"
            },
            {
              value: "2",
              label: "初级作战记录"
            },
            {
              value: "3",
              label: "中级作战记录"
            }
          ]
        },
        {
          options: [
            {
              value: "4",
              label: "赤金",
              time: 72
            }
          ]
        },
        {
          options: [
            {
              value: "5",
              label: "双芯片",
              time: 60
            }
          ]
        },
        {
          options: [
            {
              value: "6",
              label: "源石碎片(固)",
              time: 60
            },
            {
              value: "7",
              label: "源石碎片(装)",
              timt: 60
            }
          ]
        }
      ],
      key: 0,
      level: 1,
      powerU: 10,
      effectiveness: 1.01,
      moodUse: 1.0,
      options2: [],
      choose: [
        {
          time: 45,
          type: 1,
          value: 200
        },
        {
          time: 80,
          type: 1,
          value: 400
        },
        {
          time: 180,
          type: 1,
          value: 1000
        },
        {
          time: 72,
          type: 2,
          value: 500
        },
        {
          time: 60,
          type: 3,
          value: 1
        },
        {
          time: 60,
          type: 4,
          value: 10
        },
        {
          time: 60,
          type: 4,
          value: 10
        }
      ],
      value: "",
      value1: [],
      num: 0
    };
  },
  props: ["number", "powerR"],
  created: function() {
    let x;
    for (x in employee.all) {
      let a = {
        value: 0,
        label: "",
        skillName: ""
      };
      a.value = employee.all[x].id;
      a.label = employee.all[x].name;
      a.skillName = employee.all[x].skillName;
      this.options2.push(a);
    }
  },
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
      this.$emit("updateM", this.powerU, this.number);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.manufacture {
  background-color: rgba(255, 209, 93, 0.568);
  height: 140px;
  border: 1px solid black;
  padding: 5px 10px;
  margin-bottom: 3px;
  margin: 0 auto;
}
</style>