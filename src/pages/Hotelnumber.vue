<template>
  <div class="ade-top1">
    <div class="ar-top2">
      <div class="fx1">
        <h4 class="h4-hoter">验证分析</h4>
        <h3 class="h3-hoter">验证数量</h3>
      </div>
      <p class="ccc5">本页根据今日酒店验证数据来计算</p>
      <div class="zhibiao1">
        <div class="zhibiaotop1">今日核心指标</div>
        <div class="nuber">
          <ul class="ul11">
            <li class="li11">
              到店使用数
              <span class="nuber-span11">{{oneData.dcount}}</span>
            </li>
          </ul>
        </div>
      </div>
    </div>
    <el-table :data="tableData" border style="width: 100%; margin-top: 200px;">
      <el-table-column prop="checkDate" label="日期"></el-table-column>
      <el-table-column prop="dcount" label="到店使用数"></el-table-column>
      <el-table-column prop="ycount" label="预约成功数"></el-table-column>
      <el-table-column prop="hotel_name" label="店铺名称"></el-table-column>
    </el-table>
    <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page.sync="curPage"
        :page-size="pageSize"
        layout="total,prev, pager, next, jumper"
        :total="counts"
      ></el-pagination>
    <div class="tbotton-botn" style="width:80%;height:60px;overflow: hidden; margin:0 auto;margin-top: 35%;" >
      <div class="botton-ctn" style="width:100%;height:50px;margin:0 auto; ">
        <span
          style="display:block;width:200px;height:20px;margin:0 auto;text-align:center;lin-height:20px;font-size:12px;
        "
        >蜀ICP备15003730号-1</span>
        <span
          style="display:block;width:100%;height:20px;margin:0 auto;text-align: center;lin-height:20px;font-size:12px;
        "
        >@Copyright2019黑豆旅行.AllRightsReserved.</span>
      </div>
    </div>
  </div>
</template>
<script>
import Vue from "vue";
import { Table, TableColumn } from "element-ui";
Vue.use(Table);
Vue.use(TableColumn);

export default {
  data() {
    return {
      curPage:1,
      pageSize:10,
      counts: 0,
      tableData: [],
      oneData: {
        checkDate: 0,
        dcount: 0,
        ycount: 0
      }
    };
  },
  created() {
    this.getCurrentDay();
  },
  methods: {
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
     handleCurrentChange(val) {
      this.curPage = val;
      this.getCurrentDay();
    },
    getCurrentDay() {
      var that = this;
      var param = JSON.stringify({
        curPage:this.curPage,
        pageSize:this.pageSize,
        merchantNo:localStorage.getItem("username")
      });
      var params = new URLSearchParams();
      params.append("param",param);
      that.$axios.post("/backstage/Statistisc/getHotelVerifyCount",params,{}).then(
        res => {
          that.tableData = res.data.datas.list;
          that.oneData = res.data.datas.curDate;
          that.counts = res.data.datas.counts;
          console.log(res);
        },
        err => {
          console.log(err);
        }
      );
    }
  }
};
</script>

<style>
.ade-top1 {
  width: 100%;
  height: 100%;
}
.ar-top2 {
  width: 100%;
  height: 100px;
}
.fx1 {
  width: 100%;
  height: 60px;
}
.h4-hoter {
  width: 100%;
  height: 50%;
  padding-left: 10px;
  padding-top: 5px;
  color: #727272;
}
.h3-hoter {
  padding: 5px 0;
  padding-left: 10px;
  padding-bottom: 10px;
  border-bottom: 1px solid #e0e0e0;
}
.ccc5 {
  width: 100%;
  height: 60px;
  line-height: 60px;
  margin-left: 50px;
}
.zhibiao1 {
  width: 100%;
  height: 150px;
  border: 1px solid #cccccc;
}
.zhibiaotop1 {
  width: 100%;
  height: 40px;
  line-height: 40px;
  padding-left: 10px;
  background: #cccccc;
  font-weight: 600;
}
.ul11 {
  list-style: none;
  display: flex;
  width: 100%;
  height: 100px;
  margin: 20px auto;
}
.li11 {
  width: 100%;
  height: 60px;
  text-align: center;
  line-height: 30px;
  border-left: 1px solid #cccccc;
}
.ul-li1 {
  border: none;
}
.nuber-span11 {
  display: block;
  font-weight: 700;
}
</style>