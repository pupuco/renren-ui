<template>
  <el-card shadow="never" class="aui-card--fill">
    <div class="mod-demo__lunwen}">
      <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
        <el-form-item>
          <el-input v-model="dataForm.id" placeholder="id" clearable></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="getDataList()">{{ $t('query') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button type="info" @click="exportHandle()">{{ $t('export') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button v-if="$hasPermission('demo:lunwen:save')" type="primary" @click="addOrUpdateHandle()">{{ $t('add') }}</el-button>
        </el-form-item>
        <el-form-item>
          <el-button v-if="$hasPermission('demo:lunwen:delete')" type="danger" @click="deleteHandle()">{{ $t('deleteBatch') }}</el-button>
        </el-form-item>
      </el-form>
      <el-table v-loading="dataListLoading" :data="dataList" border @selection-change="dataListSelectionChangeHandle" style="width: 100%;">
        <el-table-column type="selection" header-align="center" align="center" width="50"></el-table-column>
        <el-table-column prop="id" label="" header-align="center" align="center" v-if="false"></el-table-column>
        <el-table-column prop="depart" label="所属部门" header-align="center" align="center"></el-table-column>
        <el-table-column prop="author" label="作者" header-align="center" align="center"></el-table-column>
        <el-table-column prop="title" label="论文题目" header-align="center" align="center"></el-table-column>
        <el-table-column prop="source" label="论文来源" header-align="center" align="center"></el-table-column>
        <el-table-column prop="address" label="检索网址" header-align="center" align="center"></el-table-column>
        <el-table-column prop="pubname" label="刊物名称" header-align="center" align="center"></el-table-column>
        <el-table-column prop="pubnum" label="刊号" header-align="center" align="center"></el-table-column>
        <el-table-column prop="level" label="刊物级别" header-align="center" align="center"></el-table-column>
        <el-table-column prop="page" label="起止页码" header-align="center" align="center"></el-table-column>
        <el-table-column prop="include" label="收录级别" header-align="center" align="center"></el-table-column>
        <el-table-column :label="$t('handle')" fixed="right" header-align="center" align="center" width="150">
          <template slot-scope="scope">
            <el-button v-if="$hasPermission('demo:lunwen:update')" type="text" size="small" @click="addOrUpdateHandle(scope.row.id)">{{ $t('update') }}</el-button>
            <el-button v-if="$hasPermission('demo:lunwen:delete')" type="text" size="small" @click="deleteHandle(scope.row.id)">{{ $t('delete') }}</el-button>
          </template>
        </el-table-column>
      </el-table>
      <el-pagination
        :current-page="page"
        :page-sizes="[10, 20, 50, 100]"
        :page-size="limit"
        :total="total"
        layout="total, sizes, prev, pager, next, jumper"
        @size-change="pageSizeChangeHandle"
        @current-change="pageCurrentChangeHandle">
      </el-pagination>
      <!-- 弹窗, 新增 / 修改 -->
      <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
    </div>
  </el-card>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './lunwen-add-or-update'
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      mixinViewModuleOptions: {
        getDataListURL: '/demo/lunwen/page',
        getDataListIsPage: true,
        exportURL: '/demo/lunwen/export',
        deleteURL: '/demo/lunwen',
        deleteIsBatch: true
      },
      dataForm: {
        id: ''
      }
    }
  },
  components: {
    AddOrUpdate
  }
}
</script>
