<template>
  <div class="dashboard-container">
    <div><el-button type="primary" @click="handleValue">生成</el-button></div>
    <div>
      contractProcessStep
      <el-button
        type="primary"
        icon="el-icon-plus"
        @click="addContractProcessStep"
      ></el-button>
      <el-button
        type="primary"
        icon="el-icon-minus"
        @click="delContractProcessStep"
      ></el-button>
    </div>

    <htmlcopy
      v-for="(item, index) in Bjson.contractProcessStep"
      :key="index"
      :index="index"
      :contractProcessSteps_item="item"
      @deleteIndex="del"
      @uploadData="getData"
    >
    </htmlcopy>
    <pre class="grid-content bg-purple-dark htmlcoy" v-html="htmlcopy"></pre>
    <el-input type="textarea" autosize v-model="getBjson" placeholder="JSON：">
      <template slot="prepend">JSON：</template>
    </el-input>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import { Select, Button } from "element-ui";
import htmlcopy from "./htmlcopy.vue";
export default {
  name: "Dashboard",
  /* eslint-disable vue/no-unused-components */
  components: { Select, Button, htmlcopy },
  data() {
    return {
      currentRole: "adminDashboard",
      htmlcopy: "",
      Bjson: {
        contractProcessStep: [
          {
            step_type: 1,
            approval_list: [
              {
                name: "1",
                uid: "1",
                status: -1,
              },
            ],
          },
        ],
      },
    };
  },

  methods: {
    handleValue: function () {
      // console.log("value :>> ", value);
      // this.Bjson.step_type = Number(value);
      // let aa = `[{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"},{"a":"1"},{"b":"3"}]`;
      // this.htmlcopy = JSON.stringify(JSON.parse(_json), null, 4);
      this.htmlcopy = this.syntaxHighlight(this.Bjson);
      // this.$refs.refWord.innerHTML=this.htmlcopy;
      // console.log("this.htmlcopy :>> ", this.htmlcopy);
    },

    syntaxHighlight: function (json) {
      if (typeof json != "string") {
        json = JSON.stringify(json, undefined, "  ");
      }
      // json = json
      //   .replace(/&/g, "&amp;")
      //   .replace(/</g, "&lt;")
      //   .replace(/>/g, "&gt;");
      return json.replace(
        // eslint-disable-next-line
        /("(\\u[a-zA-Z0-9]{4}|\\[^u]|[^\\"])*"(\s*:)?|\b(true|false|null)\b|-?\d+(?:\.\d*)?(?:[eE][+\-]?\d+)?)/g,
        function (match) {
          var cls = "number";
          if (/^"/.test(match)) {
            if (/:$/.test(match)) {
              cls = "key";
            } else {
              cls = "string";
            }
          } else if (/true|false/.test(match)) {
            cls = "boolean";
          } else if (/null/.test(match)) {
            cls = "null";
          }
          return '<span class="' + cls + '">' + match + "</span>";
        }
      );
    },
    addContractProcessStep: function () {
      let a = {
        step_type: 0,
        approval_list: [
          {
            name: "",
            uid: "",
            status: -1,
          },
        ],
      };
      this.Bjson.contractProcessStep.push(a);
    },
    delContractProcessStep: function () {
      this.Bjson.contractProcessStep.pop();
    },
    // delete student
    del: function (index) {
      //  not allow to delete the first
      if (index !== 0) {
        this.Bjson.contractProcessStep.splice(index, 1);
        console.log(
          "deleted:",
          JSON.stringify(this.Bjson.contractProcessSteps)
        );
      }
    },
    //  get the data from child
    getData: function (val) {
      let index = val.index;
      this.Bjson.contractProcessStep[index] = val.data;
      console.log(
        "I got the data in index:",
        JSON.stringify(this.Bjson.contractProcessStep)
      );
    },
  },
  computed: {
    ...mapGetters(["roles"]),
    getBjson: {
      get: function () {
        return JSON.stringify(this.Bjson, null, 4);
      },
      set: function (newValue) {
        this.Bjson = JSON.parse(newValue);
      },
    },
  },
};
</script>
<style rel="stylesheet/scss" lang="scss" scoped>
::v-deep .htmlcoy {
  outline: 1px solid #ccc;
  padding: 5px;
  margin: 5px;
}
::v-deep .string {
  color: green;
}
::v-deep .number {
  color: darkorange;
}
::v-deep .boolean {
  color: blue;
}
::v-deep .null {
  color: magenta;
}
::v-deep .key {
  color: red;
}
</style>
