<template>
  <div class="app-container">
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      :default-sort = "{prop: 'id', order: 'descending'}"
    >
      <el-table-column align="center" label="序号" prop="id"></el-table-column>
      <el-table-column align="center" label="用户名" prop="username"></el-table-column>
      <el-table-column align="center" :formatter="passFormat"  label="密码" prop="password"></el-table-column>
      <el-table-column align="center" label="创建时间" prop="ctime"></el-table-column>
      <el-table-column
        fixed="right"
        label="操作"
        width="100">
        <template slot-scope="scope">
          <el-button type="text" @click="update(scope.row)" size="small">修改</el-button>
          <el-button @click="del(scope.row.id)" type="text" size="small">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <div style="margin-top: 10px;text-align: center;align-items: center">
      <el-pagination
        background
        layout="prev, pager, next"
        :page-count="pages"
        v-model:current-page="currpage"
        @current-change="loadData"/>
    </div>
  </div>
</template>

<script>
import request from '@/utils/request'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      currpage:1,
      list: [],
      listLoading: false,
      pages:1
    }
  },
  mounted() {
    this.loadData()
  },
  methods: {
    passFormat(row, column) {
      //const pass = row[column.property]
      return "******";
    },
    loadData(){
      console.log(this.currpage);
      //请求接口 实现数据的查询
      this.listLoading = true
      request({
        url:"http://127.0.0.1:8080/api/user/page.do?size=10&page="+this.currpage
      }).then(res=>{
        this.list=res.data.list;
        this.pages=res.data.pages;
        this.listLoading=false;
      })
    },
    del(id){
      this.$confirm('亲，是否确认删除吗?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        //请求接口 实现数据的删除
        request({
          url:"http://127.0.0.1:8080/api/user/del.do?id="+id
        }).then(res=>{
          if(res.code==0){
            this.$message({
              type: 'success',
              message: '删除成功!'
            });
            this.loadData();
          }
        })

      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        });
      });
    },
    update(obj){
      //设置Vuex的数据
      this.$store.commit("user/setUser",obj);
      //跳转页面 路由
      this.$router.push("/userupdate");
    }
  }
}
</script>
