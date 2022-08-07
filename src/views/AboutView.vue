<template>
  <div class="about">
    <van-collapse v-model="activeNames">
      <van-collapse-item title="详细信息" name="1">
        <van-field
          label="巡视反馈重点方面"
          :value="rowData.patrolFeedbackKeyAspect"
          readonly
          type="textarea"
          rows="3"
        />
        <van-field
          label="巡视反馈重点内容"
          :value="rowData.patrolFeedbackKeyContent"
          readonly
          type="textarea"
          rows="3"
        />
        <van-field
          label="巡视反馈具体问题"
          :value="rowData.patrolFeedbackKeyProblem"
          readonly
          type="textarea"
          rows="3"
        />
        <van-field
          label="整改计划"
          :value="rowData.plan"
          readonly
          type="textarea"
        />
        <template v-if="rowData.type === 'view'">
          <van-field label="上周进度" :value="rowData.lastProgress" readonly />
          <van-field
            label="整改进度"
            :value="rowData.currentProgress"
            readonly
          />
          <van-field
            label="整改描述"
            :value="rowData.rectificationDescription"
            readonly
          />
        </template>
        <van-field
          label="责任领导"
          :value="rowData.corporationLeader"
          readonly
        />
        <van-field label="分管领导" :value="rowData.aspect" readonly />
        <van-field
          label="整改牵头部门/负责人"
          :value="rowData.supervisorName"
          readonly
          type="textarea"
          rows="3"
        />
        <van-field
          label="整改时限"
          :value="rowData.rectificationTime"
          readonly
          type="textarea"
          rows="3"
        />
      </van-collapse-item>
      <van-collapse-item
        label="整改进度录入"
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
            v-model="form.rectificationDescription"
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
