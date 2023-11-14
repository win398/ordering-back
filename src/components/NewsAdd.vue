<template>
  <div class="hello">
    <h2>添加新闻</h2>
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="80px">
      <el-form-item label="新闻标题" prop="title">
        <el-input v-model="ruleForm.title"></el-input>
      </el-form-item>
      <el-form-item label="新闻分类" prop="kindId">
        <el-select v-model="ruleForm.kindid" placeholder="请选择新闻分类">
          <el-option
            v-for="item in options"
            :key="item.KindId"
            :label="item.content"
            :value="item.KindId"
          ></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="新闻来源" prop="source">
        <el-input v-model="ruleForm.source">
        </el-input>
      </el-form-item>
      <el-form-item label="新闻编辑" prop="editor">
        <el-input v-model="ruleForm.editor">
        </el-input>
      </el-form-item>
       <el-form-item label="新闻类型" prop="type">
        <el-select v-model="ruleForm.type" placeholder="请选择新闻类型">
          <el-option label="普通新闻" value="1"/>
          <el-option label="图片新闻" value="2"/>
          <el-option label="视频新闻" value="3"/>
        </el-select>
      </el-form-item>
      <el-form-item label="新闻内容">
        <el-input
          type="textarea"
          v-model="ruleForm.content"
          :rows="10"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-upload
          ref="upload"
          action="http://localhost:8080/news20221/news/add"
          list-type="picture-card"
          :limit="1"
          :file-list="fileList"
          :data="ruleForm"
          name="file"
          :auto-upload="false"
          :before-upload="beforeAvatarUpload"
          :on-success="handleSuccess">
            <i slot="default" class="el-icon-plus"></i>
            <div slot="file" slot-scope="{file}">
              <img
                class="el-upload-list__item-thumbnail"
                :src="file.url" alt=""
              >
              <span class="el-upload-list__item-actions">
                <span
                  class="el-upload-list__item-preview"
                  @click="handlePictureCardPreview(file)"
                >
                  <i class="el-icon-zoom-in"></i>
                </span>
                <span
                  v-if="!disabled"
                  class="el-upload-list__item-delete"
                  @click="handleDownload(file)"
                >
                  <i class="el-icon-download"></i>
                </span>
                <span
                  v-if="!disabled"
                  class="el-upload-list__item-delete"
                  @click="handleRemove(file)"
                >
                  <i class="el-icon-delete"></i>
                </span>
              </span>
            </div>
          </el-upload>
          <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="">
          </el-dialog>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')"
          >立即添加</el-button
        >
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>


<script>
export default {
  name: "NewsAdd",
  data() {
    return {
      fileList: [],
      options: [],
      dialogImageUrl: '',
      dialogVisible: false,
      disabled: false,
      ruleForm: {
        tilte: "",
        kindId: 1,
        content: "",
        type: "1",
        source: "",
        editor: "",
      },
      rules: {
        title: [
          { require: true, message: "请输入新闻标题", trigger: "blur" },
          { min: 3, max: 20, message: "长度在3到20个字符", trigger: "blur" },
        ],
        kindid: [
          { require: true, message:"请选择新闻分类", trigger: "change"}
        ],
        content: [
          { require: true, message: "请输入新闻内容", trigger: "blur" },
        ],
      },
    };
  },
  created() {
    this.axios.get("kind/list").then((response) => {
      this.options = response.data;
    });
  },
  methods: {
    handleSuccess() {
      this.$message({
        message: '成功提交新闻！',
        type: 'success'
      });
      this.$router.push("/");
    },
    beforeAvatarUpload(file) {
      const isJPG = file.type === 'image/jpeg';
      const isLt2M = file.size / 1024 / 1024 < 2;

      if (!isJPG) {
        this.$message.error('上传头像图片只能是 JPG 格式!');
      }
      if (!isLt2M) {
        this.$message.error('上传头像图片大小不能超过 2MB!');
      }
      return isJPG && isLt2M;
    },
    handleRemove(file) {
        console.log(file);
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handleDownload(file) {
      console.log(file);
    },
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.$refs.upload.submit();
        } else {
          console.log("error submit!");
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
};

</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.avatar-uploader .el-upload {
    border: 1px dashed #d9d9d9;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }
  .avatar-uploader .el-upload:hover {
    border-color: #409EFF;
  }
  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 178px;
    height: 178px;
    line-height: 178px;
    text-align: center;
  }
  .avatar {
    width: 178px;
    height: 178px;
    display: block;
  }
</style>
