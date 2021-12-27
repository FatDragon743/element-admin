
<template>
  <el-row :gutter="20">
    <hr width="90%" color="blue" size="3">
    <el-col :offset="2">
      <h3>cc_list_item:{{index+1}}</h3>
    </el-col>
    <el-col :span="6" :offset="2">
      <font class="tag">name: </font>
      <el-input v-model="cc_list_item.name" placeholder="name" style="width:70%">
      </el-input>
    </el-col>
    <el-col :span="6">
      <font class="tag">uid: </font>
      <el-input v-model="cc_list_item.uid" placeholder="uid" style="width:70%"></el-input>
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
    cc_list_item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
    };
  },
  watch: {
    cc_list_item: {
      // eslint-disable-next-line
      handler(newV, oldV) {
        if (newV.length === 0) {
          return false;
        }
        this.$emit("upload_cc_list_item", {
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
  color: red;
}
</style>
