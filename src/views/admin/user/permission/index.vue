<template>
  <div>
    <el-select
      v-loading="loading"
      v-model="currentValue"
      filterable
      placeholder="请选择"
      style="width:100%"
      @change="onChange"
    >
      <el-option
        v-for="item in options"
        :key="item.id"
        :label="item.title"
        :value="item.id"
      >
      </el-option>
    </el-select>
  </div>
</template>

<script>
import { getAll as getOptions } from "@/api/admin/permission";
export default {
  name: "admin--user--Permission--index",
  props: ["value"],
  data() {
    return {
      currentValue: this.value,
      loading: false,
      options: []
    };
  },
  watch: {
    value(val) {
      this.currentValue = val;
    }
  },
  async mounted() {
    this.getOptions();
  },
  methods: {
    onChange() {
      this.$emit("input", this.currentValue);
    },
    async getOptions() {
      this.loading = true;
      const res = await getOptions({});
      this.loading = false;
      if (!res.success) {
        if (res.message) {
          this.$message({ message: res.message, type: "error" });
        }
        return;
      }
      this.options = res.data;
    }
  }
};
</script>
