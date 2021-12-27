<template>
  <el-row :gutter="20">
    <el-col :offset="2">
      <h3>approval_list_item:{{index+1}}</h3>
    </el-col>
    <el-col :span="6" :offset="2">
      <font class="tag">name: </font><el-input v-model="approval_list_item.name" placeholder="name" style="width:70%">
      </el-input>
    </el-col>
    <el-col :span="6">
     <font class="tag">uid: </font> <el-input
        v-model="approval_list_item.uid"
        placeholder="uid" style="width:70%"
      ></el-input>
    </el-col>
    <el-col :span="6">
     <font class="tag">status: </font> <el-select
        v-model="approval_list_item.status"
        placeholder="status"
      >
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        >
        </el-option>
      </el-select>
    </el-col>
  </el-row>
</template>

<script>
export default {
  props: {
    index: {
      type: Number,
      required: true,
    },
    approval_list_item:{
      type:Object,
      required: true,
      }
  },
  data() {
    return {
      options: [
        {
          value: -1,
          label: "-1",
        },
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
      ],
    };
  },
  watch: {
    approval_list_item: {
      // eslint-disable-next-line
      handler(newV, oldV) {
        if (newV.length === 0) {
          return false;
        }
        this.$emit("uploadDatalistitem", {
          index: this.index,
          data: newV,
        });
      },
      deep: true,
    },
  },
  methods: {
    del: function () {
      this.$emit("deleteIndex", this.index);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.tag {
  color:red
}
  </style>
