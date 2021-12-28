<template>
  <div class="dashboard-container">
    <div>
      <el-button type="primary" @click="handleValue">生成</el-button>
    </div>
    <div>
      <p> contractProcessStep</p>
      <el-button type="primary" icon="el-icon-plus" @click="addContractProcessStep"></el-button>
      <el-button type="primary" icon="el-icon-minus" @click="delContractProcessStep"></el-button>
    </div>
    <div>
      <p> cc_list</p>
      <el-button type="primary" icon="el-icon-plus" @click="addcclist"></el-button>
      <el-button type="primary" icon="el-icon-minus" @click="delcclist"></el-button>
    </div>
    <!-- //==================================================================================contractProcessStep -->
    <hr width="100%" color="#987cb9" size="3">
    <h1> contractProcessStep</h1>
    <htmlcopy v-for="(item, index) in Bjson.contractProcessStep" :key="'A'+index" :index="index"
      :contractProcessSteps_item="item" @deleteIndex="del" @uploadData="getData">
    </htmlcopy>
    <!-- //==================================================================================cc_list -->
    <hr width="100%" color="#987cb9" size="3">
    <h1> cc_list</h1>
    <cclist v-for="(item, index) in Bjson.cc_list" :key="'B'+index" :index="index"
      :cc_list_item="item" @deleteIndex="del" @upload_cc_list_item="getDatacclist">
    </cclist>
    <!-- //==================================================================================cpid -->
    <hr width="100%" color="#987cb9" size="3">
    <h1> cpid</h1>
    <el-row :gutter="20">
      <el-col :span="12" :offset="2">
        <font class="tag">cpid: </font>
        <el-input v-model="Bjson.cpid" placeholder="name" style="width:70%">
        </el-input>
      </el-col>
      <el-button type="primary" icon="el-icon-refresh" @click="randomString"></el-button>
    </el-row>
    <!-- //==================================================================================oId -->
    <hr width="100%" color="#987cb9" size="3">
    <h1> oId</h1>
    <el-row :gutter="20">
      <el-col :span="12" :offset="2">
        <font class="tag">oId: </font>
        <el-input v-model="Bjson.oId" placeholder="name" style="width:70%">
        </el-input>
      </el-col>
    </el-row>
    <!-- //==================================================================================name -->
    <hr width="100%" color="#987cb9" size="3">
    <h1> name</h1>
    <el-row :gutter="20">
      <el-col :span="12" :offset="2">
        <font class="tag">name: </font>
        <el-input v-model="Bjson.name" placeholder="name" style="width:70%">
        </el-input>
      </el-col>
    </el-row>
    <!-- //==================================================================================create_time -->
    <hr width="100%" color="#987cb9" size="3">
    <h1> create_time</h1>
    <el-row :gutter="20">
      <el-col :span="12" :offset="2">
        <font class="tag">create_time: </font>
        <el-input v-model="Bjson.create_time" placeholder="name" style="width:70%">
        </el-input>
      </el-col>
      <el-button type="primary" icon="el-icon-refresh" @click="getNow('create_time')">
      </el-button>
    </el-row>
    <!-- //==================================================================================update_time -->
    <hr width="100%" color="#987cb9" size="3">
    <h1> update_time</h1>
    <el-row :gutter="20">
      <el-col :span="12" :offset="2">
        <font class="tag">update_time: </font>
        <el-input v-model="Bjson.update_time" placeholder="name" style="width:70%">
        </el-input>

      </el-col>
      <el-button type="primary" icon="el-icon-refresh" @click="getNow('update_time')">
      </el-button>
    </el-row>

    <hr width="100%" color="#987cb9" size="3">
    <!-- //================================================================================== -->
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
import cclist from "./cclist.vue";
export default {
  name: "Dashboard",
  /* eslint-disable vue/no-unused-components */
  components: { Select, Button, htmlcopy, cclist },
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
        cc_list: [{ name: "胡玉霞", uid: "g4qt08w3mm" }],
        cpid: "g4qs8eeee2",
        oId: "g4qs8ebb0m",
        name: "录制二（会签）",
        create_time: 1638342672392,
        update_time: 1640167088610,
      },
    };
  },

  methods: {
    getNow: function (type) {
      this.Bjson[type] = Date.now();
    },
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
    randomString: function () {
      this.Bjson.cpid =  (Date.now() * 1000 + Math.floor(Math.random() * 1000)).toString(
        36
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
    addcclist: function () {
      let a = { name: "", uid: "" };
      this.Bjson.cc_list.push(a);
    },
    delcclist: function () {
      this.Bjson.cc_list.pop();
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
    getDatacclist: function (val) {
      let index = val.index;
      this.Bjson.cc_list[index] = val.data;
      console.log(
        "I got the data in index:",
        JSON.stringify(this.Bjson.cc_list)
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
.tag {
  color: red;
}
</style>
