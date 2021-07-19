<template>
    <div>
        <div class="StudentTheselectedcourse">
            <Divider>已选课程</Divider>

            <Table border   :columns="columns4" :data="data1">
                  <template slot-scope="{ row, index }" slot="score">
                    <span v-if="row.score == -1">暂未评分</span>
                    <span v-else-if="row.score == null">暂未评分</span>
                    <span v-else>{{row.score}}</span>
                </template>
                <template slot-scope="{ row, index }" slot="action">
                    <Button type="primary" size="small" style="margin-right: 5px" @click="tuike(row,index)">退课</Button>
                </template>
            </Table>


        </div>
    </div>
</template>

<script>
  import axios from "axios";
  export default {
    name: "StudentTheselectedcourse",
    data () {
      return {
        columns4: [
          {
            title: '课程编号',
            key: 'cno'
          },
          {
            title: '课程名称',
            key: 'cname'
          },
          {
            title: '学分',
            key: 'credit'
          },
          {
            title: '开课时间',
            key: 'startDate'
          },
          {
            title: '结课时间',
            key: 'endDate'
          },
          {
            title: '任课老师',
            key: 'tname'
          },
          {
            title: '联系电话',
            key: 'phone'
          },
          {
            title: '分数',
            slot: 'score'
          },
          {
                        title: 'Action',
                        slot: 'action',
                        width: 150,
                        align: 'center'
                    }
        ],
        data1: [
         
        ]
      }
    },
    mounted() {
      this.getHomeInfo();
    },
    methods: {
      tuike(row,index){
        console.log(row,index)
        let obj = {
          sno: row.sno,
          cno: row.cno
        }
        axios.post("/api/tuike",obj)
          .then((res) => {
            this.$Message.error("退课成功!");
            this.getHomeInfo();
          });
      },
      getHomeInfo() {
        axios.get("/api/Studentalreadyoptionalcourses")
          .then((res) => this.getHomeInfoSucc(res));
      },
      getHomeInfoSucc(res) {
        if (res.data) {
          this.data1 = res.data.result;
        }
      }

    }
  };
</script>

<style scoped lang="scss">
    .StudentTheselectedcourse{
        padding: 0px 15px;
    }

    .StudentTheselectedcourseBtn{
        text-align: right;
        padding: 18px 15px;
    }
    .StudentTheselectedcourseBtn1{
        margin-right: 15px;
    }
</style>
