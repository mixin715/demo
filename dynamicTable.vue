<template>
  <el-row>
    <el-col :span="8">
      <el-table
        :data="ptableData"
        tooltip-effect="dark"
        stripe
        style="width: 100%; margin-top: 30px"
        border
      >
        <el-table-column :label="ptableName" prop="name" :key="index" align="center">
        </el-table-column>
        <el-table-column label="权限" align="center">
          <template slot-scope="scope">
            <el-switch
              v-model="scope.row.available"
              active-text="开放权限"
              inactive-text="关闭权限"
              @change="changeStatus($event, scope.$index, ptableName)"
            >
            </el-switch>
          </template>
        </el-table-column>
      </el-table>
    </el-col>
  </el-row>
</template>

<script>
export default {
  data() {
    return {
      tdata: {},

      selectdata: [],
    };
  },
  props: ["ptableData", "ptableName"],

  methods: {
    // handleSelectionChange(selection) {
    //   this.selectdata = selection.map((item) => item.name);
    //   this.$emit("modifydata", this.selectdata);
    // },
    changeStatus($event, index, table_name) {
      this.tdata.tablename = table_name;
      this.tdata.tindex = index;
      this.tdata.available = $event;

      // console.log($event);
      // console.log(col);
      // console.log(table_name);
      this.$emit("modifydata", this.tdata);
    },
  },
};
</script>
