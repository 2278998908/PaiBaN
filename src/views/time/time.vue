<template>
  <div>
    <h3>{{ start }} -- {{ end }}</h3>
    <el-button-group>
      <el-button @click="pre">上一周</el-button>
      <el-button @click="next">下一周</el-button>
    </el-button-group>
    <div style="width: 1200px; height: 1000px" ref="ec"></div>
  </div>
</template>

<script>
import dayjs from "dayjs";
export default {
  props: ["shopid_"],
  data() {
    return {
      id: 4,
      shopid: 0,
      start: "2023-05-08",
      end: "2023-05-14",
      empid: [],
      name: [],
      time: [],
    };
  },
  async created() {
    this.shopid = this.shopid_;
  },
  async mounted() {
    // this.getinfo();
    setTimeout(async () => {
      await this.getinfo();
      this.test();
    }, 1000);
  },
  methods: {
    async getinfo() {
      //   console.log("工时", this.shopid_);
      let { data: res } = await this.$http.get(
        "/analyze/getEmpWeekTime/" + this.shopid_ + "/" + this.id
      );
      for (let i = 0; i < res.data.length; i++) {
        this.empid.push(res.data[i].mes.empId);
        this.time.push(res.data[i].mes.weekTime);
        this.name.push(res.data[i].name);
      }
      //   console.log(this.time);
    },
    clear(){
    //     var oldChart = this.echarts.getInstanceByDom(
    //     this.$refs.ec
    //   );
    //   if (oldChart) {
    //     oldChart.dispose();
    //     myChart = null
    //   }
    //   myChart = this.echarts.init(this.$refs.ec);
      var option = {
        title: {
          text: "例子",
        },
        tooltip: {
          trigger: "axis",
          title: "详细信息",
          fontFamily: "宋体",
          fontWeight: 100,
          padding: 20,
          formatter: (params) => {
            let title = "详细信息";
            let a = parseInt(params[0].name);
            let b = this.empid.indexOf(a);
            let c = this.name[b];
            let d = params[0].data;
            // return `姓名：${c} ;ID: ${a} ;工时: ${d}`
            return (
              "<span>" +
              title +
              "<span><br>" +
              "<span>ID:" +
              a +
              "<span><br>" +
              "<span>姓名:" +
              c +
              "<span><br>" +
              "<span>工时:" +
              d +
              "<span><br>"
            );
          },
        },
        legend: {
          data: ["工时"],
        },
        xAxis: {
          name: "员工ID",
          data: this.empid,
        },
        yAxis: {
          name: "工时",
        },
        series: [
          {
            name: "工时",
            type: "bar",
            data: this.time,
          },
        ],
      };

      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
    },
    test() {
      var myChart =  this.echarts.getInstanceByDom(this.$refs.ec);
      if(myChart == null){
        myChart = this.echarts.init(this.$refs.ec);
      }
    //   myChart = this.echarts.init(this.$refs.ec);

      var option = {
        title: {
          text: "例子",
        },
        tooltip: {
          trigger: "axis",
          title: "详细信息",
          fontFamily: "宋体",
          fontWeight: 100,
          padding: 20,
          formatter: (params) => {
            let title = "详细信息";
            let a = parseInt(params[0].name);
            let b = this.empid.indexOf(a);
            let c = this.name[b];
            let d = params[0].data;
            // return `姓名：${c} ;ID: ${a} ;工时: ${d}`
            return (
              "<span>" +
              title +
              "<span><br>" +
              "<span>ID:" +
              a +
              "<span><br>" +
              "<span>姓名:" +
              c +
              "<span><br>" +
              "<span>工时:" +
              d +
              "<span><br>"
            );
          },
        },
        legend: {
          data: ["工时"],
        },
        xAxis: {
          name: "员工ID",
          data: this.empid,
        },
        yAxis: {
          name: "工时",
        },
        series: [
          {
            name: "工时",
            type: "bar",
            data: this.time,
          },
        ],
      };

      // 使用刚指定的配置项和数据显示图表。
      myChart.setOption(option);
    },
    pre() {
      if (this.id - 7 > 0) {
        this.id = this.id - 7;
        this.start = dayjs(this.start).subtract(7, "day").format("YYYY-MM-DD");
        this.end = dayjs(this.end).subtract(7, "day").format("YYYY-MM-DD");
        this.empid.splice(0, this.empid.length);
        this.time.splice(0, this.time.length);
        this.name.splice(0, this.name.length);
        this.getinfo();
      } else {
        this.$message({
          message: "上一周暂无数据",
          type: "error",
        });
        return;
      }
      this.clear()
      this.test()
    },
    next() {
      this.id = this.id + 7;
      this.start = dayjs(this.start).add(7, "day").format("YYYY-MM-DD");
      this.end = dayjs(this.end).add(7, "day").format("YYYY-MM-DD");
      this.empid.splice(0, this.empid.length);
      this.time.splice(0, this.time.length);
      this.name.splice(0, this.name.length);
      //   console.log(111);
      //   console.log(this.empid);
      this.getinfo();
    //   this.clear();
      this.test();
    },
  },
};
</script>

<style>
</style>