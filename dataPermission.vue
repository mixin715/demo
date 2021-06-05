<template>
  <el-container>
    <el-header>
      <el-row type="flex">
        <el-col :span="4"
          ><el-button type="text" size="medium"
            >设置用户定制数据在服务器端保存月数</el-button
          ></el-col
        >
        <el-col :span="4">
          <el-radio-group v-model="month.cacheDeadline" size="small">
            <el-radio :label="3" @change="getLabel($event)" border>3</el-radio>
            <el-radio :label="6" @change="getLabel($event)" border>6</el-radio>
            <el-radio :label="9" @change="getLabel($event)" border>9</el-radio>
          </el-radio-group>
        </el-col>

        <el-col :span="18">
          <el-button type="primary" @click="modifyMonth" size="small"
            >修改保存月份</el-button
          >
        </el-col>
      </el-row>
    </el-header>
    <el-main>
      <dynamictab
        v-for="(item, index) in tableData"
        :ptableData="item.columns"
        :ptableName="item.tableName"
        :key="index"
        @modifydata="commitData"
      >
      </dynamictab>
    </el-main>

    <el-footer>
      <el-row type="flex" justify="center" align="center">
        <el-col :span="18">
          <el-button type="primary" @click="open">修改数据权限</el-button>
          <!-- <el-button type="primary" @click="print">打印</el-button> -->
        </el-col>
      </el-row>
    </el-footer>
  </el-container>
</template>

<script>
import dynamictab from "./dynamicTable.vue";
import axios from "axios";

export default {
  data() {
    return {
      month: 3,
      tmonth: 0,
      tdata: {
        tablename: "",
        tindex: 0,
        available: true,
      },
      tableData: [],
    };
  },
  components: {
    dynamictab,
  },
  created() {
    this.getDataPermission();
    this.getStorageTime();
  },
  methods: {
    open() {
      this.$confirm("此操作将修改开放给用户的数据权限, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$message({
            type: "success",
            message: "修改成功!",
          });
          this.postDataPermission();
          //this.getStorageTime();
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消修改",
          });
        });
    },
    modifyMonth() {
      this.$confirm("此操作将修改服务器保存用户数据的月数, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$message({
            type: "success",
            message: "修改成功!",
          });
          this.postStorageTime();
          //this.getStorageTime();
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消修改",
          });
        });
    },
    getLabel(val) {
      this.tmonth = val;
    },
    commitData(k) {
      this.tdata = k;
      // console.log(this.tdata);
      // 忘记有v-model绑定了
      // for (let i = 0; i < this.tableData.length; i++) {
      //   if (this.tdata.tablename == this.tableData[i].tname) {
      //     console.log(this.tdata.tablename);
      //     this.tableData[i].tData[this.tdata.tindex].available = this.tdata.available;
      console.log("after:", this.tableData);
      //   }
      // }
    },

    getDataPermission() {
      axios
        .get(
          "https://www.fastmock.site/mock/339c600d7c8697ca4745cdb82ac9661c/01/getDataPermission"
        )
        .then((res) => {
          this.tableData = res.data;
        });
    },
    postDataPermission() {
      axios
        .post(
          "https://www.fastmock.site/mock/339c600d7c8697ca4745cdb82ac9661c/01/postDataPermission",
          this.tableData,
          {}
        )
        .then((res) => {
          console.log(res);
        });
    },
    postStorageTime() {
      axios
        .get(
          "https://www.fastmock.site/mock/339c600d7c8697ca4745cdb82ac9661c/01/postStorageTime",
          this.tmonth,
          {}
        )
        .then((res) => {
          console.log(res);
        });
    },
    getStorageTime() {
      axios
        .get(
          "https://www.fastmock.site/mock/339c600d7c8697ca4745cdb82ac9661c/01/getStorageTime"
        )
        .then((res) => {
          this.month = res.data;
        });
    },
    print() {
      console.log(this.tmonth);
    },
  },
};
</script>

