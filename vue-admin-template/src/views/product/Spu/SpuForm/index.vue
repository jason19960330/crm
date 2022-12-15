<template>
  <div>
    <el-form ref="form" label-width="80px">
     <el-form-item label="spu名称">
      <el-input placeholder="spu名称"></el-input>
     </el-form-item>
     <el-form-item label=" 品牌" value="">
      <el-select placeholder="请选择品牌">
        <el-option label="lable" value="value"></el-option>
      </el-select>
     </el-form-item>
     <el-form-item label="SPU描述">
      <el-input type="textarea" rows="4" placeholder="描述"></el-input>
     </el-form-item>
     <el-form-item label="spu的图片">
      <!-- 上传图片：action图片上传的地址  list-type: 文件列表的类型 on-preview:图片预览的时候会出发  on-remove:当删除图片的时候会出发 
         file-list：照片墙需要展示的数据【数组：数组里面的元素务必要有name、url属性】
         on-preview：图片预览功能
         on-remove:删除图片的时候会触发
        -->
      <el-upload
          action="/dev-api/admin/product/fileUpload"
          list-type="picture-card"
          :on-preview="handlePictureCardPreview"
          :on-remove="handleRemove"
          :on-success="handlerSuccess"
          :file-list="spuImageList"
        >
          <i class="el-icon-plus"></i>
          <el-dialog :visible.sync="dialogVisible">
            <img width="100%" :src="dialogImageUrl" alt="" />
          </el-dialog>
        </el-upload>
     </el-form-item>
     <el-form-item label="销售属性">
        <el-select>
          <el-option label="lable" value="value"></el-option>
        </el-select>
        <el-button type="primary" icon="el-icon-plus"> 添加销售属性</el-button>
        <el-table style="width: 100%" border>
        <el-table-column type="index" label="序号"  width="80px" align="center"></el-table-column>
        <el-table-column label="属性名" prop="prop" width="width"></el-table-column>
        <el-table-column label="属性值名称列表" prop="prop" width="width"></el-table-column>
        <el-table-column label="操作" prop="prop" width="width"></el-table-column>
      </el-table>
     </el-form-item>
      <el-form-item >
         <el-button type="primary">保存</el-button>
         <el-button @click="$emit('changeSence',0)">取消</el-button>
      </el-form-item>
    </el-form>
  </div>
  </template>
  
  <script>
    export default{
      name:'',
      data() {
    return {
      dialogImageUrl: "",
      dialogVisible: false,

    }
  },
  methods: {
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePictureCardPreview(file) {
        this.dialogImageUrl = file.url;
        this.dialogVisible = true;
      }
    }
  }
  </script>
  
  <style scoped >
  
  </style>