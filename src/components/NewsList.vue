<template>
  <div class="hello">
    <el-table :data="tableData">
      <el-table-column prop="newsId" label="编号" width="140"></el-table-column>
      <el-table-column prop="title" label="标题" width="500"></el-table-column>
      <el-table-column prop="from" label="来源" width="140"></el-table-column>
      <el-table-column label="发表时间" width="300">
        <template slot-scope="scope">
          <i class="el-icon-time"></i>
          <span style="margin-left: 10px">{{
            scope.row.time | dateformat
          }}</span>
        </template>
      </el-table-column>
      <el-table-column fixed="right" width="150">
        <template slot="header">
          <el-input v-model="search" size="mini" placeholder="输入关键字搜索" />
        </template>
        <template slot-scope="scope">
          <el-button size="mini" @click="handleEdit(scope.$index, scope.row)"
            >编辑</el-button
          >
          <el-button size="mini" type="danger" @click="handleDelete(scope.row)"
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      style="margin-top: 10px"
      :page-size="pageSize"
      :page-count="pageCount"
      :current-page="currentPage"
      layout="sizes, prev, pager, next"
      :total="total"
      @current-change="changePage"
      @size-change="handleSizeChange"
      :page-sizes="[5, 10, 20]"
    >
    </el-pagination>
  </div>
</template>

<script>
export default {
  name: "NewsList",
  data() {
    return {
      tableData: [],
      search: "",
      pageSize: 10, //每页显示个数
      pageCount: 10, //总页数
      total: 100, //总条目数
      currentPage: 1, //当前页
    };
  },
  created() {
    this.getList();
  },
  methods: {
    handleSizeChange(val) {
      this.pageSize = val;
      this.getList();
    },
    getList() {
      this.axios
        .get(
          "news/getPageNews?current=" +
            this.currentPage +
            "&size=" +
            this.pageSize
        )
        .then((response) => {
          this.tableData = response.data.records;
          this.pageCount = response.data.pages;
          this.total = response.data.total;
        });
    },
    changePage(currentPage) {
      this.currentPage = currentPage;
      this.getList();
    },
    handleDelete(row) {
      console.log("delete:" + row.newsid);
      this.$confirm("此操作将永久删除该记录, 是否继续?", "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.axios
            .get("c?op=delete&newsid=" + row.newsid)
            .then((response) => {
              if (response.data == "ok") {
                this.$notify({
                  title: "删除成功",
                  message: "已成功删除newsID=" + row.newsid + "的新闻！",
                  type: "success",
                });
              } else {
                this.$notify.error({
                  title: "删除失败",
                  message: "删除失败，newsID=" + row.newsid,
                });
              }
              this.getList();
            });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "已取消删除",
          });
        });
    },
    handleEdit(index, row) {
      console.log("edit:" + index + "," + row.newsid);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
