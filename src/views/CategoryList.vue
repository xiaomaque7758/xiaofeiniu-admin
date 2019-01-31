<template>
  <div class="xfn-category-list">
    <el-breadcrumb>
      <el-breadcrumb-item to="/main">首页</el-breadcrumb-item>
      <el-breadcrumb-item>菜品类别管理</el-breadcrumb-item>
      <el-breadcrumb-item>类别列表</el-breadcrumb-item>
    </el-breadcrumb>
    <br>
    <el-button type="primary">添加新的菜品类别</el-button>
    <br>
    <el-table :data="categoryList" stripe border>
      <el-table-column label="编号" prop="cid"></el-table-column>
      <el-table-column label="名称" prop="cname"></el-table-column>

      <el-table-column label="操作">
        <template slot-scope="{row,$index}"> 
            <el-button @click="deleteCategory(row,$index)" type="success" size="mini">修改</el-button>
            <el-button @click="updateCategory(row,$index)" type="error" size="mini">删除</el-button>
        </template>
      </el-table-column>

    </el-table>
  </div>
</template>

<script>
export default {
  data(){
    return {
      categoryList:[]
    }
  },
  mounted(){
    var url=this.$store.state.globalSettings.apiUrl+'/admin/category';
    this.$axios.get(url).then((res)=>{
      this.categoryList=res.data;
    }).catch((err)=>{
      console.log(err);
    })
  },
  methods:{
    deleteCategory(row,$index){
      var url=this.$store.state.globalSettings.apiUrl+'/admin/category'+row.cid;
      this.$axios.delete(url).then((res)=>{
          if(res.data.code==200){
            //数据库中已经删除成功
            this.categoryList.splice(row,1);   //模型数据中删除
            this.$message.success('菜品类别删除成功');
          }else{
            this.$message.error('类别删除出差错:'+res.data.msg);
          }
      }).catch((err)=>{
        console.log(err);
      })
    },
    updateCategory(row,$index){
       console.log(row);
       console.log($index);
    } 
  }
}
</script>

