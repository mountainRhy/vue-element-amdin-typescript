<template>
  <div id="user">
    <el-row
      style="margin-bottom: 30px;"
      :gutter="20"
    >
      <el-col :span="12">
        <el-row :gutter="10">
          <el-col :span="6">
            <el-select
              v-model="type"
              placeholder="请选择"
            >
              <el-option
                v-for="item in findTypeList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              />
            </el-select>
          </el-col>
          <el-col :span="10">
            <el-input
              v-model="userId"
              placeholder="请输入内容"
            />
          </el-col>
          <el-col :span="8">
            <el-button
              :style="{background: color, borderColor: color}"
              type="primary"
            >
              查询
            </el-button>
          </el-col>
        </el-row>
      </el-col>
    </el-row>
    <el-table
      :data="tableData"
      style="width: 100%"
    >
      <el-table-column
        label="日期"
        width="180"
      >
        <template slot-scope="scope">
          <i class="el-icon-time" />
          <span style="margin-left: 10px">{{ scope.row.date }}</span>
        </template>
      </el-table-column>
      <el-table-column
        label="姓名"
        width="180"
      >
        <template slot-scope="scope">
          <el-popover
            trigger="hover"
            placement="top"
          >
            <p>姓名: {{ scope.row.name }}</p>
            <p>住址: {{ scope.row.address }}</p>
            <div
              slot="reference"
              class="name-wrapper"
            >
              <el-tag size="medium">
                {{ scope.row.name }}
              </el-tag>
            </div>
          </el-popover>
        </template>
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="handleEdit(scope.$index, scope.row)"
          >
            编辑
          </el-button>
          <el-button
            size="mini"
            type="danger"
            @click="handleDelete(scope.$index, scope.row)"
          >
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'
import { SettingsModule } from '@/store/modules/settings'

@Component({
  name: 'User'
})
export default class extends Vue {
  private findTypeList = [
    {
      label: '手机号',
      value: 'mobile'
    },
    {
      label: '用户ID',
      value: 'userId'
    }
  ];

  private tableData = [
    {
      date: '2016-05-02',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1518 弄'
    },
    {
      date: '2016-05-04',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1517 弄'
    },
    {
      date: '2016-05-01',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1519 弄'
    },
    {
      date: '2016-05-03',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1516 弄'
    }
  ];

  private type = 'userId';

  private userId = '';

  created() {}

  get color() {
    return SettingsModule.theme
  }

  private handleEdit(index: number, row: object) {
    console.log(index, row)
  }

  private handleDelete(index: number, row: object) {
    console.log(index, row)
  }
}
</script>

<style lang="scss" scoped>
#user {
  box-sizing: border-box;
  padding: 20px;
}
</style>
