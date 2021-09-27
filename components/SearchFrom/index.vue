<template>
  <div class="search-form-box">
    <el-form :model="formData" ref="formRef" :inline="true" label-width="100px">
      <!-- 自定义插槽，可用于特殊表单块 -->
      <slot name="first"></slot>
      <el-form-item
        v-for="(item, index) in formOptions"
        :key="newKeys[index]"
        :prop="item.prop"
        :label="item.label ? item.label + '：' : ''"
        :rules="item.rules"
      >
        <from-items v-model="formData[item.prop]" :itemOptions="item" />
      </el-form-item>

      <!-- 自定义插槽，可用于特殊表单块 -->
      <slot name="last"></slot>
    </el-form>

    <!-- 提交按钮 -->
    <div class="btn-box">
      <el-button
        v-if="btnItems.includes('search')"
        type="success"
        class="btn-search"
        @click="onSearch"
        >搜 索</el-button
      >

      <el-button
        v-if="btnItems.includes('export')"
        type="primary"
        class="btn-export"
        @click="onExport"
        >导 出</el-button
      >

      <el-button
        v-if="btnItems.includes('reset')"
        type="primary"
        class="btn-reset"
        @click="onReset"
        >重 置</el-button
      >
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import FromItems from './FromItems';
export default {
  components: { FromItems },
  props: {
    /**
     * 表单配置
     * 示例：
     * [{
     *   label: '用户名', // label文字
     *   prop: 'username', // 字段名
     *   element: 'el-input', // 指定elementui组件
     *   initValue: '阿黄', // 字段初始值
     *   placeholder: '请输入用户名', // elementui组件属性
     *   rules: [{ required: true, message: '必填项', trigger: 'blur' }], // elementui组件属性
     *   events: { // elementui组件方法
     *     input (val) {
     *       console.log(val)
     *     },
     *     ...... // 可添加任意elementui组件支持的方法
     *   }
     *   ...... // 可添加任意elementui组件支持的属性
     * }]
     */
    formOptions: {
      type: Array,
      required: true,
      default() {
        return [];
      },
    },
    // 提交按钮项，多个用逗号分隔（search, export, reset）
    btnItems: {
      type: String,
      default() {
        return "search";
      },
    },
  },

  data() {
    return {
      formData: {},
    };
  },

  computed: {
    newKeys() {
      return this.formOptions.map((v) => {
        let createUniqueString = function createUniqueString() {
          const timestamp = +new Date() + "";
          const randomNum = parseInt((1 + Math.random()) * 65536) + "";
          return (+(randomNum + timestamp)).toString(32);
        };
        return createUniqueString();
      });
    },
  },

  created() {
    this.addInitValue();
  },

  methods: {
    // 校验
    onValidate(callback) {
      this.$refs.formRef.validate((valid) => {
        if (valid) {
          callback();
        }
      });
    },
    // 搜索
    onSearch:_.throttle(function(e) {
      this.onValidate(() => {
        this.$emit("onSearch", this.formData);
      });
    },1000),
    // 导出
    onExport() {
      this.onValidate(() => {
        this.$emit("onExport", this.formData);
      });
    },
    onReset() {
      this.$refs.formRef.resetFields();
      this.$emit("onReset");
    },
    // 添加初始值
    addInitValue() {
      const obj = {};
      this.formOptions.forEach((v) => {
        if (v.initValue !== undefined) {
          obj[v.prop] = v.initValue;
        }
      });
      this.formData = obj;
    },
  },
};
</script>

<style scoped>
.search-form-box {
  
}
.btn-box {
  margin-top: 15px;
  text-align: center;
}
.el-form {
}
.el-form-item__label {
  padding-right: 0;
}
.el-form-item {
  /* margin-bottom: 0; */
  margin-right: 20px;
}
.el-select {
  width: 120px;
}
.el-cascader {
  width: 200px;
}
</style>