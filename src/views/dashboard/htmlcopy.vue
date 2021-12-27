<template>
  <div>
    <hr width="90%" color="blue" size="3">
    <h2>contractProcessStep_item:{{ index + 1 }}</h2>
    <el-row :gutter="20">
      <el-col :span="6">
        <span class="tag">step_type: </span>
        <el-select v-model="contractProcessSteps_item.step_type" placeholder="step_type">
          <el-option v-for="i in options1" :key="i.value" :label="i.label" :value="i.value">
          </el-option>
        </el-select>
      </el-col>
      <el-col :span="6">
        <span class="tag">approval_list: </span>
        <el-select v-model="approval_list_length" placeholder="approval_list">
          <el-option v-for="item in options2" :key="item.value" :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </el-col>
    </el-row>
    <hr width="80%" color="red" size="3">
    <el-row>
      <listitem v-for="(
          approval_list_item, index
        ) in contractProcessSteps_item.approval_list" :key="index" :index="index"
        :approval_list_item="approval_list_item" @deleteIndex="del" @uploadDatalistitem="getData">
      </listitem>
    </el-row>

  </div>
</template>

<script>
import listitem from "@/views/dashboard/listitem";
export default {
  components: { listitem },
  props: {
    index: {
      type: Number,
      required: true,
    },
    contractProcessSteps_item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      options1: [
        {
          value: 1,
          label: "1",
        },
        {
          value: 2,
          label: "2",
        },
        {
          value: 3,
          label: "3",
        },
        {
          value: 4,
          label: "4",
        },
        {
          value: 5,
          label: "5",
        },
      ],
      options2: [
        {
          value: 1,
          label: "1",
        },
        {
          value: 2,
          label: "2",
        },
        {
          value: 3,
          label: "3",
        },
        {
          value: 4,
          label: "4",
        },
        {
          value: 5,
          label: "5",
        },
      ],
      approval_list: [],
    };
  },
  watch: {
    contractProcessSteps_item: {
      // eslint-disable-next-line
      handler(newV, oldV) {
        if (newV.length === 0) {
          return false;
        }
        // console.log('object :>> ', newV.step_type);
        this.$emit("uploadData", { index: this.index, data: newV });
      },
      deep: true,
    },
  },
  methods: {
    del: function () {
      this.$emit("deleteIndex", this.index);
    },
    getData: function (val) {
      let index = val.index;
      this.approval_list[index] = val.data;
      this.contractProcessSteps_item.approval_list = this.approval_list;
      console.log(
        "I got the data in htmlcopy:",
        JSON.stringify(this.approval_list)
      );
    },
  },
  computed: {
    approval_list_length: {
      get: function () {
        return this.contractProcessSteps_item.approval_list.length;
      },
      set: function (val) {
        console.log("val :>> ", val);
        let approval_list_length =
          this.contractProcessSteps_item.approval_list.length;
        if (val > approval_list_length) {
          for (let i = approval_list_length; i < val; i++) {
            this.contractProcessSteps_item.approval_list.push({
              name: "",
              uid: "",
              status: -1,
            });
          }
        } else {
          this.contractProcessSteps_item.approval_list =
            this.contractProcessSteps_item.approval_list.slice(0, val);
        }
        console.log(
          "this.approval_list :>> ",
          this.contractProcessSteps_item.approval_list.length
        );
      },
    },
  },
};
</script>
<style scoped>
.tag {
  color: red;
}
</style>
