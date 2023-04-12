<!-- 自己向别人申请 -->
<template>
  <div>
    <el-table :data="msg" border>
      <el-table-column prop="data" label="全部信息"> </el-table-column>
      <el-table-column prop="status" label="状态"> </el-table-column>
    </el-table>
    <!-- <div style="width: 1200px;height: 1000px;" ref="ec"></div> -->
  </div>
</template>

<script>
export default {
  props:["shopid_"],
  data() {
    return {
      msg: [],
      empid:[],
      name:[],
      time:[]
    };
  },
  async created() {
    const token = localStorage.getItem("token");
    let b;
    const a = await this.$http.get("/apply/getBeApplied/" + token);
    console.log(a.data.data);
    for (let i = 0; i < a.data.data.length; i++) {
      if (a.data.data[i].status == 3) {
        b = "暂未处理";
      } else if (a.data.data[i].status == 2) {
        b = "被拒绝";
      } else {
        b = "同意";
      }
      this.msg.push({
        data:
          "我想把" +
          a.data.data[i].exchangeDate +
          "日" +
          a.data.data[i].exchangeStartTime +
          "到" +
          a.data.data[i].exchangeEndTime +
          "的班次和" +
          a.data.data[i].exchangedName +
          "" +
          a.data.data[i].exchangedDate +
          "日" +
          a.data.data[i].exchangedStartTime +
          "到" +
          a.data.data[i].exchangedEndTime +
          "的班次交换",
        status: b,
      });
    }
    //this.msg = a.data.data
  },
  // mounted(){
  //   this.test()
  // },
  // methods:{
  //   async test(){
  //       let {data:res} = await this.$http.get('/analyze/getEmpWeekTime/'+this.shopid_+'/4')
  //       for(let i = 0 ; i < res.data.length ; i ++){
  //         this.empid.push(res.data[i].mes.empId)
  //         this.time.push(res.data[i].mes.weekTime)
  //         this.name.push(res.data[i].name)
  //       }
  //       var myChart = this.echarts.init(this.$refs.ec);
 
  //       var option = {
  //           title: {
  //               text: '例子'
  //           },
  //           tooltip: {
  //             trigger:'axis',
  //             title:'详细信息',
  //             fontFamily:'宋体',
  //             fontWeight: 100,
  //             padding: 20,
  //             formatter:(params)=>{
  //               let title = '详细信息'
  //               let a = parseInt(params[0].name)
  //               let b = this.empid.indexOf(a)
  //               let c = this.name[b]
  //               let d = params[0].data
  //               // return `姓名：${c} ;ID: ${a} ;工时: ${d}`
  //               return '<span>'+title+'<span><br>'+'<span>ID:'+a+'<span><br>'+'<span>姓名:'+c+'<span><br>'+'<span>工时:'+d+'<span><br>'
  //             }
  //           },
  //           legend: {
  //               data:['工时']
  //           },
  //           xAxis: {
  //               name:'员工ID',
  //               data: this.empid
  //           },
  //           yAxis: {
  //             name:'工时'
  //           },
  //           series: [{
  //               name: '工时',
  //               type: 'bar',
  //               data: this.time
  //           }]
  //       };
 
  //       // 使用刚指定的配置项和数据显示图表。
  //       myChart.setOption(option);
  //   }
  // }
};
</script>

<style>
</style>