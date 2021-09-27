<template>
  <div class="search-table-box">
    <el-table :data="tableData" border style="width: 100%" v-loading="loading"  @selection-change="handleSelectionChange">
        <el-table-column v-if="selection" type="selection" width="40"></el-table-column>
        <el-table-column v-for="(th, index) in tableItem" :key="index" :prop="th.prop" v-bind="bindProp(th)" :label="th.label" align="center">
            <template slot-scope="{ row }">
                <span v-if="th.link" class="router-link" @click="router(th.to,row)">{{row[th.prop]}}</span>
                <span v-else-if="th.filter">{{th.filterFun(row[th.prop])}}</span>
                <span v-else>{{row[th.prop]}}</span>
            </template>
        </el-table-column>
        <el-table-column v-if="isAction" label="操作" align="center" fixed="right">
            <template slot-scope="{ row }">
                <el-button v-for="(button, index) in actionButtonList" :key="index" v-bind="bindButton(button)" @click="clickButton(button.mark,row)">{{button.text}}</el-button>
            </template>
        </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name:'SearchTable',
  props: {
    tableData: {
      type: Array,
      default() {
        return []
      },
    },
    tableItem: {
      type: Array,
      required: true,
      default() {
        return []
      },
    },
    selection: {
      type: Boolean,
      default() {
        return false
      }
    },
    actionButtonList: {
      type: Array,
      default() {
        return []
      },
    },
    loading: {
      type: Boolean,
      default() {
        return false
      },
    }
  },
  data() {
    return {
      
    };
  },

  computed: {
    // 判断有无操作按钮项
    isAction() {
      if (this.actionButtonList.length) {
        return true
      } else {
        return false
      }
    }
  },
  methods: {
    // 点击按钮
    clickButton(mark,row) {
      this.$emit('action',mark,row)
    },
    // 点击复选框
    handleSelectionChange(val) {
      this.$emit('selectChange',val)
    },
    // 表格参数
    bindProp(item) {
      let obj = { ...item }
      // 移除冗余属性
      delete obj.label
      delete obj.prop
      delete obj.link
      delete obj.to
      delete obj.filter
      delete obj.filterFun
      return obj
    },
    // 按钮参数
    bindButton(item) {
      let obj = { ...item }
      // 移除冗余属性
      delete obj.text
      delete obj.mark
      return obj
    },
    // 连接跳转匹配{}中的数值
    router(to,row) {
      const path = to.path.replace(/{(.+?)\}/g, (result, key) => {
        const value = row[key]
        return value.toString()
      })
      this.$router.push({path:path})
    }
  },
};
</script>

<style scoped>
.router-link{
  color: rgb(24, 144, 255);
  cursor: pointer;
  text-decoration: underline;
}
</style>