<template>
  
  <div>
    <!-- 三级联动全局组件 -->
    <el-card style="margin: 20px 0px">
      <CategorySelect @getCategoryId="getCategoryId" :show="scene != 0"></CategorySelect>
    </el-card>
    <el-card>

      <div v-show="scene == 0">
      <!-- 添加SPU的按钮 -->

      <el-button type="primary" icon="el-icon-plus" :disabled="!category3Id" @click="addSpu"> 添加spu</el-button>
      <el-table style="width: 100%" border :data="records">
        <el-table-column type="index" label="序号" width="80" align="center">

        </el-table-column>
        <el-table-column label="SPU名称" prop="spuName" width="width"></el-table-column>
        <el-table-column label="SPU属性" prop="description" width="width"></el-table-column>
        <el-table-column label="操作" prop="prop" width="width">
          <template slot-scope="{row,$index}">
            <!-- 未来会更换 -->
           <hint-button type="success" icon="el-icon-plus" size="mini" title="添加sku"></hint-button>
           <hint-button type="warning" icon="el-icon-edit" size="mini" title="修改sku" @click="updateSpu(row)"></hint-button>
           <hint-button type="info" icon="el-icon-info" size="mini" title="查看当前spu全部sku列表"></hint-button>
           <!-- 删除按钮的回调 -->
           <el-popconfirm
           title="这是一段内容确定删除吗？"
           @onConfirm="deleteSpu(row)"
         >
           <hint-button
             type="danger"
             icon="el-icon-delete"
             size="mini"
             title="删除spu"
             slot="reference"
           ></hint-button>
         </el-popconfirm>
          </template>
        </el-table-column>
        
      </el-table>
      <!--  @current-change="getSpuList"
          -->
      <el-pagination
         style="text-align:center"
          :current-page="page"
          :page-sizes="[3, 5, 10]"
          :page-size="limit"
          layout="prev, pager, next, jumper,->, sizes,total"
          @current-change="getSpuList"
          @size-change="handleSizeChange"
          :total="total">

      </el-pagination>
      </div>
       <SpuForm v-show="scene==1" @changeScene="changeScene" ref="spu"></SpuForm>
       <SkuForm v-show="scene==2">2</SkuForm>

    </el-card>
  </div>
</template>

<script>
  //引入子组件
import SpuForm from "./SpuForm";
import SkuForm from "./SkuForm";
  export default {
    name: 'Spu',
    data() {
      return {
        //分别是分类的id
        category1Id: "",
        category2Id: "",
        category3Id: "",
        //控制级联动菜单
        
        page: 1, //分页器当前第几页
       limit: 3, //每一页需要展示多少条数据
       records: [], //spu列表的数据
       total:0,//分页器一共需要展示数据的条数
       scene: 0, //0代表展示SPU列表数据   1 添加SPU|修改SPU   2 添加SKU
      }
    },
    methods: {
      //点击分页器的第几页的按钮
      // handleSizeChange(page){
      //   this.page=page;
      //   this.getSpuList();

      // },
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
      },
      //获取SPU列表的数据方法
     async getSpuList(pages=1){
      this.page=pages;
        const {page,limit,category3Id}=this;
        //携带三个参数 page:第几页 limit 每一页展示多少条数据， 三级分类的id 
      let result= await this.$API.spu.reqSpuList(page,limit,category3Id);
      if(result.code==200){
        this.total=result.data.total;
        this.records=result.data.records;
      }
      },
      //当分页器的某一个展示数据条数发生变化时的回调
      handleSizeChange(limit){
        //修改参数
        this.limit=limit;
        //再发请求
        this.getSpuList();
      },
      //添加SPU按钮的回调
      addSpu(){
      //切换场景为1
      this.scene=1;
      //通知子组件SpuFom发请求----两个
      this.$refs.spu.addSpuData(this.category3Id);

      },
     //修改某一个SPU
    updateSpu(row) {
      this.scene = 1;
      //获取子组件SpuForm子组件的
      //在父组件当中可以通过$ref获取子组件等等
      this.$refs.spu.initSpuData(row);
    },
 //自定义回调事件（spufrom）
   //自定义事件回调（SpuForm）
   changeScene({ scene, flag }) {
      //flag这个形参为了区分保存按钮是添加还是修改
      //切换结构（场景）
      this.scene = scene;
      //子组件通知父组件切换场景，需要再次获取SPU列表的数据进行展示
      if (flag == "修改") {
        this.getSpuList(this.page);
      } else {
        this.getSpuList();
      }
    },
    //删除SPU的回调
    async deleteSpu(row){
      let result=await this.$API.spu.reqDeleteSpu(row.id);
      if(result.code==200){
        this.$message({type:"success",message:"删除成功"});
        //代表SPU个数大于1删除的时候停留在当前页，如果SPU个数小于1 回到上一页
        this.getSpuList(this.records.length>1?this.page:this.page-1);
      }
    },

    },

    components: {
    SpuForm,
    SkuForm,
  },
  }
</script>

<style scoped>

</style>