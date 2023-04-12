<!-- 别人申请自己 -->
<template>
  <div>
    <h3 class="loding">待处理</h3>
    <el-table :data="msg" border ref="table">
      <el-table-column
        prop="applyId"
        label="交换ID"
        width="100px"
        align="center"
      >
      </el-table-column>
      <el-table-column prop="data" label="全部信息"> </el-table-column>
      <el-table-column prop="status" label="状态" align="center">
      </el-table-column>
      <el-table-column prop="" label="操作" width="220px" align="center">
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="warning"
            icon="el-icon-edit"
            :ref="'a'+scope.$index"
            @click="agree(scope.row,scope.$index)"
            >同意</el-button
          >
          <el-button
            size="mini"
            type="danger"
            icon="el-icon-delete"
            :ref="'r'+scope.$index"
            @click="refuse(scope.row,scope.$index)"
            >拒绝</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <h3 class="loding">已处理</h3>
    <el-table :data="old" border ref="table">
      <el-table-column
        prop="applyId"
        label="交换ID"
        width="100px"
        align="center"
      >
      </el-table-column>
      <el-table-column prop="data" label="全部信息"> </el-table-column>
      <el-table-column prop="status" label="状态" align="center">
      </el-table-column>
    </el-table>
    <button @click="a(0)">测试</button>
  </div>
</template>

<script>
let token = 0;
export default {
  props:["shopid_"],
  data() {
    return {
      msg: [],
      old:[]
    };
  },
  async created() {
    token = localStorage.getItem("token");
    let b;
    const a = await this.$http.get("/apply/getApply/" + token);
    console.log(a.data.data);
    for (let i = 0; i < a.data.data.length; i++) {
      if (a.data.data[i].status == 3) {
        b = "暂未处理";
        this.msg.push({
        data:
          a.data.data[i].exchangeName +
          "想把" +
          a.data.data[i].exchangeDate +
          "日" +
          a.data.data[i].exchangeStartTime +
          "到" +
          a.data.data[i].exchangeEndTime +
          "的班次和我的" +
          "" +
          a.data.data[i].exchangedDate +
          "日" +
          a.data.data[i].exchangedStartTime +
          "到" +
          a.data.data[i].exchangedEndTime +
          "的班次交换",
        status: b,
        applyId: a.data.data[i].applyId,
      });
      } else if (a.data.data[i].status == 2) {
        b = "拒绝";
        this.old.push({
        data:
          a.data.data[i].exchangeName +
          "想把" +
          a.data.data[i].exchangeDate +
          "日" +
          a.data.data[i].exchangeStartTime +
          "到" +
          a.data.data[i].exchangeEndTime +
          "的班次和我的" +
          "" +
          a.data.data[i].exchangedDate +
          "日" +
          a.data.data[i].exchangedStartTime +
          "到" +
          a.data.data[i].exchangedEndTime +
          "的班次交换",
        status: b,
        applyId: a.data.data[i].applyId,
      });
      } else {
        b = "同意";
        this.old.push({
        data:
          a.data.data[i].exchangeName +
          "想把" +
          a.data.data[i].exchangeDate +
          "日" +
          a.data.data[i].exchangeStartTime +
          "到" +
          a.data.data[i].exchangeEndTime +
          "的班次和我的" +
          "" +
          a.data.data[i].exchangedDate +
          "日" +
          a.data.data[i].exchangedStartTime +
          "到" +
          a.data.data[i].exchangedEndTime +
          "的班次交换",
        status: b,
        applyId: a.data.data[i].applyId,
      });
      }
    }
  },
  methods: {
    async agree(row,index) {
      console.log(this.$refs.table);
      const a = await this.$http.put(
        "/apply/delExchange/" + row.applyId + "/1"
      );
      console.log(a.data);
      let b = 'a'+index
      let c = 'r'+index
      this.$refs[b].disabled = true
      this.$refs[c].disabled = true
      // location.reload()
    },
    async refuse(row,index) {
      const a = await this.$http.put(
        "/apply/delExchange/" + row.applyId + "/2"
      );
      console.log(a);
      let b = 'a'+index
      let c = 'r'+index
      this.$refs[b].disabled = true
      this.$refs[c].disabled = true
    //   location.reload();
    },
    a(c){
        console.log(this.$refs)
        let b='a'+c
        this.$refs[b].disabled = true
        console.log(this.$refs[b],{b})
    }
  },
};
</script>

<style>
.loding {
    margin: 10px 0;
}
</style>