<template>
  <div>
    <!-- 三级联动全局组件 -->
    <el-card style="margin: 20px 0px">
      <CategorySelect @getCategoryId="getCategoryId" :show="!show"></CategorySelect>
    </el-card>
    <el-card>
      <!-- 添加SPU的按钮 -->
      <el-button type="primary" icon="el-icon-plus"> 添加spu</el-button>
      <el-table width="100%" border>
        <el-table-column type="index" label="序号"  width="80" align="center">

        </el-table-column>
        <el-table-column label="SPU名称" prop="prop" width="width"></el-table-column>
        <el-table-column label="SPU属性" prop="prop" width="width"></el-table-column>
        <el-table-column label="操作" prop="prop" width="width">
          <template slot-scope="{row,$index}">
            <!-- 未来会更换 -->
           <el-button type="success" icon="el-icon-plus" size="mini"></el-button>
           <el-button type="warning" icon="el-icon-edit" size="mini"></el-button>
           <el-button type="info" icon="el-icon-info" size="mini"></el-button>
           <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
          </template>
        </el-table-column>

      </el-table>
      <!--  @current-change="getSpuList"
          @size-change="handleSizeChange" -->
      <el-pagination
         style="text-align:center"
          :current-page="6"
          :page-sizes="[3, 5, 10]"
          :page-size="3"
          layout="prev, pager, next, jumper,->, sizes,total"
          :total="total">

      </el-pagination>
    </el-card>
  </div>
</template>

<script>
  export default {
    name: 'Spu',
    data() {
      return {
        //分别是分类的id
        category1Id: "",
        category2Id: "",
        category3Id: "",
        //控制级联动菜单
        show: true,
      }
    },
    methods: {
      getCategoryId({ categoryId, level }) {
        //categoryId:获取到一、二、三级分类的id  level：为了区分是几级id
        if (level == 1) {
          this.category1Id = categoryId;
          //清除2,3级分类的id
          this.category2Id = "";
          this.category3Id = "";
        } else if (level == 2) {
          this.category2Id = categoryId;
          //清除三级id
          this.category3Id = "";
        } else {
          this.category3Id = categoryId;
          //获取SPU列表数据进行展示
          this.getSpuList();
        }
      }
    },
  }
</script>

<style scoped>

</style>