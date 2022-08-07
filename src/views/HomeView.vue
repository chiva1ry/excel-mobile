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
            :desc="item.rectificationPlan"
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
import json from "./data.js";

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
      tableData: null,
    };
  },
  created() {
    this.tableData = json.data.records;
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
