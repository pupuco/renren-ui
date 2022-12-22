<template>
  <el-dialog :visible.sync="visible" :title="!dataForm.id ? $t('add') : $t('update')" :close-on-click-modal="false" :close-on-press-escape="false">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmitHandle()" :label-width="$i18n.locale === 'en-US' ? '120px' : '80px'">
          <el-form-item label="所属部门" prop="depart">
          <el-input v-model="dataForm.depart" placeholder="所属部门"></el-input>
      </el-form-item>
          <el-form-item label="作者" prop="author">
          <el-input v-model="dataForm.author" placeholder="作者"></el-input>
      </el-form-item>
          <el-form-item label="论文题目" prop="title">
          <el-input v-model="dataForm.title" placeholder="论文题目"></el-input>
      </el-form-item>
          <el-form-item label="论文来源" prop="source">
          <el-input v-model="dataForm.source" placeholder="论文来源"></el-input>
      </el-form-item>
          <el-form-item label="检索网址" prop="address">
          <el-input v-model="dataForm.address" placeholder="检索网址"></el-input>
      </el-form-item>
          <el-form-item label="刊物名称" prop="pubname">
          <el-input v-model="dataForm.pubname" placeholder="刊物名称"></el-input>
      </el-form-item>
          <el-form-item label="刊号" prop="pubnum">
          <el-input v-model="dataForm.pubnum" placeholder="刊号"></el-input>
      </el-form-item>
          <el-form-item label="刊物级别" prop="level">
          <el-input v-model="dataForm.level" placeholder="刊物级别"></el-input>
      </el-form-item>
          <el-form-item label="起止页码" prop="page">
          <el-input v-model="dataForm.page" placeholder="起止页码"></el-input>
      </el-form-item>
          <el-form-item label="收录级别" prop="include">
          <el-input v-model="dataForm.include" placeholder="收录级别"></el-input>
      </el-form-item>
      </el-form>
    <template slot="footer">
      <el-button @click="visible = false">{{ $t('cancel') }}</el-button>
      <el-button type="primary" @click="dataFormSubmitHandle()">{{ $t('confirm') }}</el-button>
    </template>
  </el-dialog>
</template>

<script>
import debounce from 'lodash/debounce'
export default {
  data () {
    return {
      visible: false,
      dataForm: {
        id: '',
        depart: '',
        author: '',
        title: '',
        source: '',
        address: '',
        pubname: '',
        pubnum: '',
        level: '',
        page: '',
        include: ''
      }
    }
  },
  computed: {
    dataRule () {
      return {
        depart: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        author: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        title: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        source: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        address: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        pubname: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        pubnum: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        level: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        page: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ],
        include: [
          { required: true, message: this.$t('validate.required'), trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    init () {
      this.visible = true
      this.$nextTick(() => {
        this.$refs['dataForm'].resetFields()
        if (this.dataForm.id) {
          this.getInfo()
        }
      })
    },
    // 获取信息
    getInfo () {
      this.$http.get(`/demo/lunwen/${this.dataForm.id}`).then(({ data: res }) => {
        if (res.code !== 0) {
          return this.$message.error(res.msg)
        }
        this.dataForm = {
          ...this.dataForm,
          ...res.data
        }
      }).catch(() => {})
    },
    // 表单提交
    dataFormSubmitHandle: debounce(function () {
      this.$refs['dataForm'].validate((valid) => {
        if (!valid) {
          return false
        }
        this.$http[!this.dataForm.id ? 'post' : 'put']('/demo/lunwen/', this.dataForm).then(({ data: res }) => {
          if (res.code !== 0) {
            return this.$message.error(res.msg)
          }
          this.$message({
            message: this.$t('prompt.success'),
            type: 'success',
            duration: 500,
            onClose: () => {
              this.visible = false
              this.$emit('refreshDataList')
            }
          })
        }).catch(() => {})
      })
    }, 1000, { 'leading': true, 'trailing': false })
  }
}
</script>
