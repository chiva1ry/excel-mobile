<template>
  <div class="about">
    <van-collapse v-model="activeNames">
      <van-collapse-item title="详细信息" name="1">
        <van-cell-group>
          <van-cell title="巡视反馈重点方面" :value="rowData.aspect" readonly />
          <van-cell
            title="巡视反馈重点内容"
            :value="rowData.content"
            readonly
          />
          <van-cell
            title="巡视反馈具体问题"
            :value="rowData.problem"
            readonly
          />
          <van-cell title="整改计划" :value="rowData.plan" readonly />
          <template v-if="rowData.type === 'view'">
            <van-cell title="上周进度" :value="rowData.lastProgress" readonly />
            <van-cell
              title="整改进度"
              :value="rowData.currentProgress"
              readonly
            />
            <van-cell title="整改描述" :value="rowData.description" readonly />
          </template>
          <van-cell title="责任领导" :value="rowData.corpLeader" readonly />
          <van-cell title="分管领导" :value="rowData.aspect" readonly />
          <van-cell
            title="整改牵头部门/负责人"
            :value="rowData.aspect"
            readonly
          />
          <van-cell title="整改时限" :value="rowData.aspect" readonly />
        </van-cell-group>
      </van-collapse-item>
      <van-collapse-item
        title="整改进度录入"
        name="2"
        v-if="rowData.type === 'edit'"
      >
        <van-form @submit="onSubmit">
          <van-field
            label="上周进度 %"
            :value="rowData.lastProgress"
            readonly
          />
          <van-field
            v-model="form.currentProgress"
            clearable
            required
            type="digit"
            label="整改进度 %"
            placeholder="请输入整改进度"
            :rules="[
              { validator, message: '整改进度范围为0-100' },
              { required: true, message: '请输入整改进度' },
            ]"
          />
          <van-field
            v-model="form.description"
            rows="3"
            autosize
            label="整改描述"
            type="textarea"
            maxlength="250"
            error-message=""
            placeholder="请输入整改描述"
            show-word-limit
            clearable
            required
            :rules="[{ required: true, message: '请输入整改描述' }]"
          />
          <div style="margin: 16px">
            <van-button
              round
              block
              type="info"
              :loading="loading"
              native-type="submit"
            >
              提交
            </van-button>
          </div>
        </van-form>
      </van-collapse-item>
    </van-collapse>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: false,
      activeNames: ["1", "2"],
      rowData: null,
      form: {
        currentProgress: "",
        description: "",
        planId: null,
      },
      type: null,
    };
  },
  created() {
    this.rowData = this.$route.params;
    this.type = this.$route.type;
    this.form.planId = this.$route.params.planId;
    console.log(this.$route.params);
  },
  mounted() {},
  methods: {
    validator(val) {
      return /(?:\b|-)([1-9]{1,2}[0]?|100)\b/.test(val);
    },
    onSubmit() {
      this.loading = true;
      console.log("submit", this.form);
      this.$router.push("/");
    },
  },
};
</script>
