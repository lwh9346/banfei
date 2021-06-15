<template>
  <v-app>
    <v-main>
      <v-app-bar color="#C5E1A5">
        <v-toolbar-title>班费展示板</v-toolbar-title>
        <v-spacer></v-spacer>
      </v-app-bar>
      <v-container>
        <v-card>
          <v-card-title>班费概览</v-card-title>
          <v-card-text
            ><v-row
              ><v-col
                ><v-progress-circular
                  size="100"
                  :value="(100 * (total - used)) / total"
                  rotate="-90"
                  color="#0091EA"
                  >{{ total - used }}/{{ total }}</v-progress-circular
                ></v-col
              >
              <v-col
                ><p>学期班费总计：{{ total }}</p>
                <p>学期班费使用：{{ used }}</p>
                <p>学期班费剩余：{{ total - used }}</p></v-col
              >
            </v-row></v-card-text
          >
        </v-card>
        <v-card style="margin-top: 24px">
          <v-card-title>班费明细</v-card-title>
          <v-card-text
            ><v-data-table :headers="table_headers" :items="table_items">
            </v-data-table
          ></v-card-text> </v-card
      ></v-container>
    </v-main>
  </v-app>
</template>

<script>
import Axios from "axios";

export default {
  name: "App",

  components: {},

  data: () => ({
    table_headers: [
      { text: "名称", value: "name" },
      { text: "发起人", value: "user" },
      { text: "日期", value: "date" },
      { text: "数额", value: "amount" },
      { text: "描述", value: "description" },
    ],
    table_items: [],
    total: 3200.0,
    used: 0.0,
  }),

  watch: {
    table_items(newVal, oldVal) {
      let tmp = 0;
      newVal.forEach((element) => {
        tmp += element.amount;
      });
      this.used = tmp;
    },
  },

  mounted() {
    Axios.get("info.json").then((resp) => {
      const data = resp.data;
      this.table_items = data;
    });
  },
};
</script>
