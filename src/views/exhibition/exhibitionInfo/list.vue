<template>
  <div class="app-container">
    <!-- 表单查询 -->
    <el-form :inline="true" class="demo-form-inline">
      <el-form-item>
        <el-input v-model="searchObj.exName" placeholder="名称" clearable/>
      </el-form-item>
      <el-form-item>
        <el-input v-model="searchObj.exCode" placeholder="编号" clearable/>
      </el-form-item>
      <el-button type="primary" icon="el-icon-search" @click="getList()">查询</el-button>
      <!--      <el-button type="danger" @click="removeRows()">批量删除</el-button>-->
      <router-link :to = "'/exhibition/exhibitionInfo/add/'">
        <el-button type="primary" icon="el-icon-edit">添加</el-button>
      </router-link>

    </el-form>

    <!--    <el-table-->
    <!--      :data="list"-->
    <!--      stripe-->
    <!--      style="width: 100%"-->
    <!--      @selection-change="handleSelectionChange">-->
    <!--      <el-table-column type="selection" width="55"/>-->

    <!--    <div>-->
    <!--      <el-button type="danger" size="mini" @click="removeRows()">批量删除</el-button>-->
    <!--    </div>-->

    <!-- banner列表 -->
    <!--    <el-table-->
    <!--      :data="list"-->
    <!--      stripe-->
    <!--      style="width: 100%"-->
    <!--      @selection-change="handleSelectionChange">-->

    <!--      <el-table-column type="selection" width="55"/>-->
    <!--      <el-table-column type="index" width="50" label="序号"/>-->

    <!--      <el-table-column prop="exName" label="名称"/>-->
    <!--      <el-table-column prop="exCode" label="编号"/>-->
    <!--      <el-table-column prop="exSite" label="地点"/>-->
    <!--      <el-table-column prop="exVenue" label="场馆"/>-->
    <!--      <el-table-column prop="exST" label="开始时间" width="150"/>-->
    <!--      <el-table-column prop="exET" label="结束时间" width="150"/>-->
    <!--      <el-table-column prop="exRequire" label="参展商要求" width="150"/>-->
    <!--      <el-table-column prop="exRates" label="收费标准"/>-->
    <!--      <el-table-column prop="exRoom" label="食宿安排"/>-->
    <!--      <el-table-column prop="exOrganization" label="会展组织机构" width="150"/>-->
    <!--      &lt;!&ndash;      <el-table-column prop="apiUrl" label="api基础路径"width="200"/>&ndash;&gt;-->
    <!--      &lt;!&ndash;      <el-table-column label="状态" width="80">&ndash;&gt;-->
    <!--      &lt;!&ndash;        <template slot-scope="scope">&ndash;&gt;-->
    <!--      &lt;!&ndash;          {{ scope.row.status === 1 ? '可用' : '不可用' }}&ndash;&gt;-->
    <!--      &lt;!&ndash;        </template>&ndash;&gt;-->
    <!--      &lt;!&ndash;      </el-table-column>&ndash;&gt;-->

    <el-table
      :data="list"
      style="width: 100%">
      <el-table-column type="expand">
        <template slot-scope="props">
          <el-form label-position="left" inline class="demo-table-expand">
            <el-form-item label="场馆">
              <span>{{ props.row.exVenue }}</span>
            </el-form-item>
            <el-form-item label="开始时间">
              <span>{{ props.row.exST }}</span>
            </el-form-item>
            <el-form-item label="结束时间">
              <span>{{ props.row.exET }}</span>
            </el-form-item>
            <el-form-item label="参展商要求">
              <span>{{ props.row.exRequire }}</span>
            </el-form-item>
            <el-form-item label="收费标准">
              <span>{{ props.row.exRates }}</span>
            </el-form-item>
            <el-form-item label="食宿安排">
              <span>{{ props.row.exRoom }}</span>
            </el-form-item>
            <el-form-item label="会展组织机构">
              <span>{{ props.row.exOrganization }}</span>
            </el-form-item>
          </el-form>
        </template>
      </el-table-column>

      <el-table-column
        label="编号"
        prop="exCode"
        width="80"/>
      <el-table-column
        label="名称"
        prop="exName"/>
      <el-table-column
        label="地点"
        prop="exSite"/>

      <el-table-column label="操作" width="280" align="center">
        <template slot-scope="scope">
          <el-button
            type="danger"
            size="mini"
            icon="el-icon-delete"
            @click="removeDataById(scope.row.id)"/>
          <router-link :to = "'/exhibition/exhibitionInfo/edit/' + scope.row.id">
            <el-button type="primary" size="mini" icon="el-icon-edit"/>
          </router-link>
        </template>
      </el-table-column>

    </el-table>

    <!-- 分页 -->
    <el-pagination
      :current-page="current"
      :page-size="limit"
      :total="total"
      style="padding: 30px 0; text-align: center;"
      layout="total, prev, pager, next, jumper"
      @current-change="getList"
    />

  </div>
</template>

<style>
.demo-table-expand {
  font-size: 0;
}
.demo-table-expand label {
  width: 100px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
}
</style>
<script>
// 引入接口文件
// import exhibitionInfoSet from '../../../api/exhibitionInfoSet'
import exhibitionInfoSet from '@/api/exhibitionInfoSet'

export default {

  // 定义变量和初始值
  data() {
    return {
      current: 1, // 当前页
      limit: 10, // 每页显示记录数
      searchObj: {}, // 条件封装对象
      list: [], // 每页数据集合
      total: 0 // 总记录数
    }
  },
  created() { // 在页面渲染之前执行
    // 一般调用methods内定义的方法得到数据
    this.getList()
  },
  methods: {// 定义方法请求接口调用
    // 获取列表
    getList(page = 1) { // 添加当前页参数
      this.current = page
      exhibitionInfoSet.getExhibitionSetList(this.current, this.limit, this.searchObj)
        .then(respose => { // respose返回接口参数
          this.list = respose.data.records
          this.total = respose.data.total
          console.log(respose)
        })// 成功后执行
        .catch(error => {
          console.log(error)
        })// 失败后执行
    },
    // 删除医院设置的方法
    removeDataById(id) {
      this.$confirm('此操作将永久此条展会信息, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => { // 确定执行then方法
        // 调用接口
        exhibitionInfoSet.deleteExhibitionSet(id)
          .then(response => {
            // 提示
            this.$message({
              type: 'success',
              message: '删除成功!'
            })
            // 刷新页面
            this.getList(1)
          })
      })
    },
    // 当表格复选框选项发生变化的时候触发
    handleSelectionChange(selection) {
      this.multipleSelection = selection
    },
    // 批量删除
    removeRows() {
      this.$confirm('此操作将永远删除这些展会信息, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => { // 确定执行then方法
        var idList = []
        // 遍历数组得到每个id值，设置到idList里面
        for (var i = 0; i < this.multipleSelection.length; i++) {
          var obj = this.multipleSelection[i]
          var id = obj.id
          idList.push(id)
        }
        // 调用接口
        exhibitionInfoSet.batchExhibitionSet(idList)
          .then(response => {
            // 提示
            this.$message({
              type: 'success',
              message: '删除成功!'
            })
            // 刷新页面
            this.getList(1)
          })
      })
    }

  }

}
</script>
