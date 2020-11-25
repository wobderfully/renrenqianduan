<template>
    <el-card shadow="never" class="aui-card--fill">
        <div class="mod-sys__dept">
            <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
                <el-form-item>
                    <el-input v-model="dataForm.name" placeholder="请输入部门名称" clearable></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button @click="getDataList()">{{ $t('query') }}</el-button>
                </el-form-item>
                <el-form-item>
                    <el-button
                        v-if="$hasPermission('sys:dept:save')"
                        type="primary"
                        @click="addOrUpdateHandle()"
                    >{{ $t('add') }}</el-button>
                </el-form-item>
            </el-form>
            <el-table
                v-loading="dataListLoading"
                :data="dataList"
                row-key="id"
                border
                style="width: 100%;"
            >
                <el-table-column
                    prop="name"
                    :label="$t('dept.name')"
                    header-align="center"
                    min-width="150"
                ></el-table-column>
                <el-table-column
                    prop="parentName"
                    :label="$t('dept.parentName')"
                    header-align="center"
                    align="center"
                ></el-table-column>
                <el-table-column
                    prop="type"
                    label="分类"
                    header-align="center"
                    align="center"
                    width="100"
                >
                    <template slot-scope="scope">
                        <span v-if="scope.row.type=='FACTORY'">厂区</span>
                        <span v-if="scope.row.type=='DEPARTMENT'">部门</span>
                        <span v-if="scope.row.type=='OTHER'">其他</span>
                    </template>
                </el-table-column>
                <el-table-column
                    prop="leader"
                    label="负责人"
                    header-align="center"
                    align="center"
                    width="150"
                ></el-table-column>
                <el-table-column
                    prop="sort"
                    :label="$t('dept.sort')"
                    header-align="center"
                    align="center"
                    width="80"
                ></el-table-column>
                <el-table-column
                    :label="$t('handle')"
                    fixed="right"
                    header-align="center"
                    align="center"
                    width="150"
                >
                    <template slot-scope="scope">
                        <el-button
                            v-if="$hasPermission('sys:dept:update')"
                            type="text"
                            size="small"
                            @click="addOrUpdateHandle(scope.row.id)"
                        >{{ $t('update') }}</el-button>
                        <el-button
                            v-if="$hasPermission('sys:dept:delete')"
                            type="text"
                            size="small"
                            @click="deleteHandle(scope.row.id)"
                        >{{ $t('delete') }}</el-button>
                    </template>
                </el-table-column>
            </el-table>
            <!-- 弹窗, 新增 / 修改 -->
            <add-or-update
                v-if="addOrUpdateVisible"
                ref="addOrUpdate"
                @refreshDataList="getDataList"
            ></add-or-update>
        </div>
    </el-card>
</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './dept-add-or-update'
export default {
  mixins: [mixinViewModule],
  data () {
    return {
      dataForm: {
        name: ''
      },
      mixinViewModuleOptions: {
        getDataListURL: '/sys/dept/list',
        deleteURL: '/sys/dept'
      }
    }
  },
  components: {
    AddOrUpdate
  }
}
</script>
