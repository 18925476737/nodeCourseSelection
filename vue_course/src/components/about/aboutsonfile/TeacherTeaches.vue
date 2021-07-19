<template>
  <div>
    <div class="TeacherTeachestable">
      <Divider>我的任教课程</Divider>
      <Table :columns="columns1" :data="data1">
        <template slot-scope="{ row, index }" slot="action">
          <Button
            type="primary"
            size="small"
            style="margin-right: 5px"
            @click="seeMyStudent(row, index)"
            >查看我的学生</Button
          >
        </template>
      </Table>
    </div>
    <Drawer
      :closable="true"
      width="840"
      v-model="showTable"
    >
      <Divider>我的课程学生</Divider>
      <Table :columns="columns2" :data="data2">
        <template slot-scope="{ row, index }" slot="score">
          <span v-if="row.score == -1">暂未评分</span>
          <span v-else-if="row.score == null">暂未评分</span>
          <span v-else>{{ row.score }}</span>
        </template>
        <template slot-scope="{ row, index }" slot="action">
          <Button
            type="primary"
            size="small"
            style="margin-right: 5px"
            @click="commentSorce(row, index)"
            >评分</Button
          >
        </template>
      </Table>
    </Drawer>
    <Drawer title="学生分数" :closable="false" v-model="showPinfeng">
      <Input v-model="score" placeholder="请输入分数" type="number" />
      <Button
        type="primary"
        size="small"
        style="margin-right: 5px; margin-top: 10px"
        @click="commitsorce()"
        >提交</Button
      >
    </Drawer>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "TeacherTeaches",
  data() {
    return {
      showTable: false,
      showPinfeng: false,
      columns1: [
        {
          title: "课程编号",
          key: "cno",
        },
        {
          title: "课程名",
          key: "cname",
        },
        {
          title: "学分",
          key: "credit",
        },
        {
          title: "开课时间",
          key: "startDate",
        },
        {
          title: "结课时间",
          key: "endDate",
        },
        {
          title: "操作",
          slot: "action",
        },
      ],
      data1: [],
      columns2: [
        {
          title: "学生编号",
          key: "sno",
        },
        {
          title: "学生姓名",
          key: "sname",
        },
        {
          title: "联系电话",
          key: "phone",
        },
        {
          title: "分数",
          slot: "score",
        },
        {
          title: "操作",
          slot: "action",
        },
      ],
      data2: [],
      tempsno: -1,
      tempcno: -1,
      score:""
    };
  },
  mounted() {
    this.getHomeInfo();
  },
  methods: {
    commentSorce(row, index) {
      this.tempsno = row.sno
      this.showPinfeng = true;
    },
    commitsorce(){
      console.log(this.tempsno,this.tempcno,this.score)
      let obj = {
        sno: this.tempsno,
        cno: this.tempcno,
        score: this.score
      };
      axios.post("/api/addscore", obj).then((res) => {
         this.seeMyStudent(obj);
         this.showPinfeng = false
      });
    },
    seeMyStudent(row) {
      console.log(row);
      this.tempcno = row.cno
      let obj = {
        cno: row.cno,
      };
      axios.post("/api/getStudengByCourse", obj).then((res) => {
        this.showTable = true;
        if (res.data) {
          this.data2 = res.data.result;
        }
      });
    },
    getHomeInfo() {
      let username = {
        username: sessionStorage.getItem("username"),
      };
      axios
        .post("/api/TeacherFindCourse", username)
        .then((res) => this.getHomeInfoSucc(res));
    },
    getHomeInfoSucc(res) {
      console.log(res.data);

      if (res.data) {
        this.data1 = res.data.result;
      }
    },
  },
};
</script>

<style scoped lang="scss">
.TeacherTeachestable {
  padding: 0 15px;
}
</style>
