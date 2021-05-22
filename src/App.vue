<template>
  <div id="app">
    <div class="wl-gantt-demo">
      <wlGantt
        ref="wl-gantt-demo"
        use-real-time
        end-date="2019-11-02"
        start-date="2019-9-06"
        date-type="monthAndDay"
        :edit="false"
        :defaultExpandAll="true"
        :data="data"
        :columns="columns"
        :contextMenuOptions="contextMenuOptions"
        @selection-change="selectionChange"
        @expand-change="expandChange"
        @timeChange="timeChange"
        @taskRemove="taskRemove"
        @preChange="preChange"
      >
        <template slot="prv">
          <!-- <el-table-column type="selection" width="55" align="center"> </el-table-column> -->
          <el-table-column type="index" width="55" label="序号">
          </el-table-column>
        </template>
        <template #info-card="{ row }">
          <ul>
            <li>
              <label for="name">名称：</label
              ><span id="name">{{ row.name }}</span>
            </li>
          </ul>
        </template>
      </wlGantt>
    </div>
  </div>
</template>

<script>
import WlGantt from "@/pages/wl-gantt";

export default {
  name: "app",
  data() {
    return {
      projectTime: {},
      data: [
        {
          id: "1", //自身id
          name: "工序A", //工序名称
          startDate: "2019-09-01", //开始时间
          endDate: "2019-10-31", //结束时间
          children: [
            {
              id: "1-1", //自身id
              pid: "1", //工序id  需跟工序id保持一致
              name: "产线A", //设备名称
              startDate: "2019-09-01", //开始时间
              endDate: "2019-09-01", //结束时间
              process: "1",
              // 白班
              day: [
                {
                  wo_no: 1, //工单号
                  part_no: 1, //零件号
                  part_desc: "xxx", //零件描述
                  plan_shift: "x", //排产班次
                  plan_qty: 1, //数量
                  ratio: 0.2, //占比
                  type: "", //类型
                },
                {
                  wo_no: 2, //工单号
                  part_no: 2, //零件号
                  part_desc: "xxx", //零件描述
                  plan_shift: "x", //排产班次
                  plan_qty: 1, //数量
                  ratio: 0.4, //占比
                  type: "计划订单", //类型
                },
                {
                  wo_no: 2, //工单号
                  part_no: 2, //零件号
                  part_desc: "xxx", //零件描述
                  plan_shift: "x", //排产班次
                  plan_qty: 1, //数量
                  ratio: 0.3, //占比
                  type: "计划订单", //类型
                },
              ],
              // 晚班
              night: [
                {
                  wo_no: 1,
                  part_no: 1,
                  desc: "xxx",
                  plan_shift: "x",
                  plan_qty: 1,
                  ratio: 0.3,
                  type: "计划订单",
                },
              ],
            },
          ],
        },
        {
          id: "2",
          name: "租房子",
          startDate: "2019-09-20",
          endDate: "2019-10-31",
        },
      ], // 数据
      selected: [], // 选中数据
      contextMenuOptions: [
        { label: "任务名称", prop: "name" },
        { label: "开始时间", prop: "startDate" },
        { label: "结束时间", prop: "endDate" },
      ],
      columns: [{ type: "name", minWidth: 200, colType: "expand" }], // 可通过此参数配置列。其中内置有名称name、开始日期startDate、结束日期endDate、前置任务preTask，如果cloumns中有type等于这四个且slot为false时，将使用内置代码，当然除了内容使用内置代码，其他字段你还拥有配置权。另外如果不是为了配置内置列参数，slot中的prv和default仍可以用来自定义列
    };
  },
  methods: {
    aa(row) {
      // eslint-disable-next-line no-console
      console.log(row, 99);
    },
    /**
     * 时间发生更改
     * row: Object 当前行数据c
     */
    timeChange(row) {
      // eslint-disable-next-line no-console
      console.log("时间修改:", row);
    },
    //
    /**
     * 前置任务发生更改
     * row: Object 当前行数据
     * oldval: [String, Array] 前置修改前的旧数据
     * handle: Boolean 是否用户编辑产生的改变
     */
    preChange(row, oldval, handle) {
      // eslint-disable-next-line no-console
      console.log("前置修改:", row, oldval, handle);
    },
    // 数表展开行
    expandChange(row, expanded) {
      // eslint-disable-next-line no-console
      console.log("展开行:", row, expanded);
    },
    // 多选选择
    selectionChange(/* val */) {
      // console.log("多选：", val);
    },
    // 删除任务
    taskRemove(item) {
      // eslint-disable-next-line no-console
      console.log("删除任务：", item);
    },
    // 添加任务
    taskAdd(item) {
      // eslint-disable-next-line no-console
      console.log("添加任务：", item);
      // 非懒加载方式直接设置子数据
      /* this.$set(
        item,
        "children",
        item.children.concat([
          {
            pid: item.id,
            id: "###",
            name: "一轮新月",
            startDate: "2019-10-11",
            endDate: "2019-10-19"
          }
        ])
      ); */
      this.$refs["wl-gantt-demo"].loadTreeAdd(item.id, [
        {
          pid: item.id,
          id: "###",
          name: "一轮新月",
          startDate: "2019-10-11",
          endDate: "2019-10-19",
        },
      ]);
    },
    // 懒加载
    lazyLoad(tree, treeNode, resolve) {
      setTimeout(() => {
        resolve([
          {
            id: "1-1-1",
            pid: tree.id,
            name: "日落云巅",
            startDate: "2019-09-10",
            endDate: "2019-09-13",
          },
        ]);
      }, 1000);
    },
  },
  components: {
    WlGantt,
  },
};
</script>

<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  padding: 20px 25px 0;
}

.wl-gantt-demo {
  margin: 40px auto;
  // width: 800px;
}
</style>
