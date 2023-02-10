<template>
  <div class="content">
    <div class="body">
      <div class="banner">
        <client-only>
          <el-input
            placeholder="请输入内容"
            v-model="input"
            class="input-with-select"
          >
            <el-button
              @click="getValue"
              slot="append"
              :loading="loading"
              icon="el-icon-search"
            ></el-button>
          </el-input>
        </client-only>
      </div>
      <div class="value" v-html="value"></div>
    </div>
  </div>
</template>
<script>
import { Message } from "element-ui";
export default {
  props: {},
  data() {
    return {
      input: "",
      loading: false,
      value: "",
    };
  },
  methods: {
    async getValue() {
      this.loading = true;
      let res = await this.$axios({
        url: "/chat_test",
        params: { input: this.input },
        method: "get",
      });
      if (res.data.code) {
        Message.success(res.data.msg);
        this.loading = false;
        this.value = res.data.response.choices[0].text;
      } else {
        Message.error(res.data.msg);
      }
      console.log(res, "res");
    },
  },
};
</script>
<style lang='scss' scoped>
.content {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}
.body {
  width: 90%;
  height: 90%;
  border: 1px solid rgb(210, 207, 207);
}
.banner {
  height: 80px;
}
.el-input {
  width: 50%;
  margin: 20px;
}
.value {
  width: 96%;
  height: 85%;
  box-sizing: border-box;
  margin: 20px;
  border: 1px solid rgb(210, 207, 207);
  color: gray;
  padding: 10px;
}
</style>