<template>
  <el-container>
    <el-main >
        <el-table
          :data="showTable"
          stripe
          style="width: 30%; margin-top: 30px"
          border
        >
          <el-table-column
            prop="userId"
            label="用户id"
            align="center"
            width="180"
          >
          </el-table-column>
          <el-table-column
            prop="createTime"
            label="下载时间"
            align="center"
            width="212"
          >
          </el-table-column>
          <el-table-column
            prop="sourceTable"
            label="下载表格"
            align="center"
            width="210"
          >
          </el-table-column>
        </el-table>
    </el-main>

    <el-footer>

        <el-pagination
          @current-change="handleCurrentChange"
          layout="total, prev, pager, next"
          :page-size="pagesize"
          :total="this.tableData.length"
          background
        >
        </el-pagination>
    </el-footer>
  </el-container>
</template>

<script>
// import { createAction } from "@antv/g2/lib/interaction/action";
import axios from "axios";
export default {
  data() {
    return {
      pagesize: 10, //设置每页显示条目个数为10
      currentPage: 1, //设置当前页默认为1

      tableData: [], //分页前的数据
    };
  },
  created() {
    this.getDownloadHistory();
  },
  computed: {
    //showTable计算属性通过slice方法计算表格当前应显示的数据
    showTable() {
      return this.tableData.slice(
        (this.currentPage - 1) * this.pagesize,
        this.currentPage * this.pagesize
      );
    },
  },
  methods: {},

  methods: {
    //设置当前页为点击页
    handleCurrentChange(val) {
      console.log("当前页: ${val}");
      console.log(val);
      this.currentPage = val;
    },
    getDownloadHistory() {
      axios
        .get(
          "http://10.128.211.150:8765/globalHistory"
        )
        .then((res) => {
          this.tableData = res.data;
        });
    },
    print() {
      console.log(this.currentPage);
    },
  },
};
</script>