<template>
  <div class="home">
    <van-pull-refresh v-model="refreshing" @refresh="onRefresh">
      <van-list
        v-model="loading"
        :finished="finished"
        finished-text="没有更多了"
        @load="onLoad"
      >
        <van-swipe-cell
          v-for="item in tableData"
          :key="item.id"
          style="margin-bottom: 16px"
        >
          <van-card
            num="2"
            :desc="item.plan"
            title="整改计划"
            class="goods-card"
          >
            <template #price>
              <div>上周进度 %</div>
              <div>整改进度 %</div>
            </template>
            <template #num>
              <div>{{ item.lastProgress || "-" }}</div>
              <div>{{ item.currentProgress || "-" }}</div>
            </template>
          </van-card>

          <template #right>
            <van-button
              square
              type="primary"
              text="详情"
              @click="editData(item, 'view')"
            />
            <van-button
              square
              type="info"
              text="编辑"
              @click="editData(item, 'edit')"
            />
          </template>
        </van-swipe-cell>
      </van-list>
    </van-pull-refresh>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "HomeView",
  components: {},
  data() {
    return {
      list: [],
      loading: false,
      finished: false,
      refreshing: false,
      rowData: null,
      title: "查看计划",
      type: "edit",
      visible: false,
      formData: null,
      selectRow: null,
      showEdit: false,
      tableData: [
        {
          planId: 100001,
          aspect:
            "巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面巡视反馈重点方面",
          content:
            "巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容巡视反馈重点内容",
          problem:
            "巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题巡视反馈具体问题",
          plan: "整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划整改计划",
          description: "整改描述",
          corpLeader: "责任领导",
          branchLeader: "分管领导",
          department: "整改牵头部门",
          supervisor: "责任人",
          lastProgress: 43,
          currentProgress: 55,
          rectificationTime: "整改时限",
          lockStatus: "该数据是否锁定不可编辑",
          versionName: "版本名称",
          versionId: "版本id",
        },
        {
          planId: 100002,
          aspect: "巡视反馈重",
          content: "巡视反馈重",
          problem: "巡视反馈具体问题巡",
          plan: "整改计划",
          description: null,
          corpLeader: "责任领导",
          branchLeader: "分管领导",
          department: "整改牵头部门",
          supervisor: "责任人",
          lastProgress: 100,
          currentProgress: null,
          rectificationTime: "整改时限",
          lockStatus: "该数据是否锁定不可编辑",
          versionName: "版本名称",
          versionId: "版本id",
        },
        {
          planId: 100003,
          aspect: "巡视反馈重",
          content: "巡视反馈重",
          problem: "巡视反馈具体问题巡",
          plan: "整改计划",
          description: null,
          corpLeader: "责任领导",
          branchLeader: "分管领导",
          department: "整改牵头部门",
          supervisor: "责任人",
          lastProgress: null,
          currentProgress: null,
          rectificationTime: "整改时限",
          lockStatus: "该数据是否锁定不可编辑",
          versionName: "版本名称",
          versionId: "版本id",
        },
      ],
    };
  },
  mounted() {
    console.log("initialize");
  },
  methods: {
    editData(data, type) {
      data.type = type;
      console.log(data);
      this.rowData = data;
      this.$router.push({ name: "about", params: data });
      this.type = type;
    },
    handleOk() {
      this.visible = false;
    },
    handleCancel() {
      this.visible = false;
    },
    onLoad() {
      setTimeout(() => {
        if (this.refreshing) {
          this.list = [];
          this.refreshing = false;
        }
        for (let i = 0; i < 10; i++) {
          this.list.push(this.list.length + 1);
        }
        this.loading = false;

        if (this.list.length >= 40) {
          this.finished = true;
        }
      }, 1000);
    },
    onRefresh() {
      // 清空列表数据
      this.finished = false;

      // 重新加载数据
      // 将 loading 设置为 true，表示处于加载状态
      this.loading = true;
      this.onLoad();
    },
  },
};
</script>

<style scoped>
.van-button {
  height: 100%;
}
</style>
