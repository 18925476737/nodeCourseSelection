<template>
    <div class="Addthecourse">
        <div class="main">
            <div class="AddthecourseForm">
                <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
                    <Divider>课程信息录入</Divider>
                    <FormItem label="课程名称" prop="coursetitle">
                        <Input v-model="formValidate.coursetitle" placeholder="请输入课程名称" show-word-limit maxlength="12"/>
                    </FormItem>

                    <FormItem label="学分" prop="credit">
                        <Input v-model="formValidate.credit" placeholder="请输入学分" show-word-limit maxlength="4"/>
                    </FormItem>


                    <FormItem label="开课日期">
                        <div class="AddthecourseDate">
                            <FormItem prop="date">
                                <DatePicker type="date" placeholder="选择日期"
                                            v-model="formValidate.beginsdate"></DatePicker>
                            </FormItem>
                            -
                            <FormItem prop="time">
                                <TimePicker type="time" placeholder="选择日期"
                                            v-model="formValidate.beginstime"></TimePicker>
                            </FormItem>
                        </div>
                    </FormItem>


                    <FormItem label="结课日期">
                        <div class="AddthecourseDate">
                            <FormItem prop="date">
                                <DatePicker type="date" placeholder="选择日期"
                                            v-model="formValidate.endsdate"></DatePicker>
                            </FormItem>
                            -
                            <FormItem prop="time">
                                <TimePicker type="time" placeholder="选择日期"
                                            v-model="formValidate.endstime"></TimePicker>
                            </FormItem>
                        </div>
                    </FormItem>

                    <FormItem label="授课教师" prop="GiveLessonsTeacher">
                        <Select v-model="formValidate.GiveLessonsTeacher" placeholder="请选择授课教室">
                            <Option v-for="item in teacherList" :value="item.value" :key="item.value">{{ item.label }}</Option>
                    
                        </Select>
                    </FormItem>

                    <FormItem>
                        <Button type="primary" @click="handleSubmit('formValidate')">Submit</Button>
                        <Button @click="handleReset('formValidate')" style="margin-left: 8px">Reset</Button>
                    </FormItem>
                </Form>
            </div>

        </div>
    </div>
</template>

<script>

  import axios from "axios";

  export default {
    name: "Addthecourse",
    data() {
      return {
        formValidate: {
          coursetitle: "",
          credit:'',
          beginsdate: "",
          beginstime:"",
          endsdate:"",
          endstime:"",
          time: "",
          GiveLessonsTeacher:""
        },
        teacherList:[],
        ruleValidate: {
          coursetitle: [
            { required: true, message: "课程名称不能为空", trigger: "blur" }
          ],
          credit: [
            { required: true, message: "学分不能为空", trigger: "blur" }
          ],

          beginsdate: [
            { required: true, type: "date", message: "日期不能为空", trigger: "change" }
          ],
          beginstime: [
            { required: true, type: "string", message: "日期不能为空", trigger: "change" }
          ],

          endsdate: [
            { required: true, type: "date", message: "日期不能为空", trigger: "change" }
          ],
          endstime: [
            { required: true, type: "string", message: "日期不能为空", trigger: "change" }
          ],
          GiveLessonsTeacher: [
            { required: true, message: "授课教室不能为空", trigger: "blur" }
          ]
        }
      };
    },
    created(){
      this.getTeacher()

    },
    methods: {
      getTeacher(){
         axios.get("/api/getTeacher")
          .then((res) => {
            console.log(res,"----")
            this.teacherList = res.data.result.map((element,index) =>{
              return {
                value: element.tname,
                label: element.tname
              }
            })
          });
      },
      handleSubmit(credit) {
        this.$refs[credit].validate((valid) => {
          if (valid) {
            this.getHomeInfo()
          } else {
            this.$Message.error("Fail!");
          }
        });
      },
      handleReset(credit) {
        this.$refs[credit].resetFields();
      },

      getHomeInfo() {
        let obj={
          coursetitle:this.formValidate.coursetitle,
          credit:this.formValidate.credit,
          beginsdate:this.formValidate.beginsdate,
          endsdate:this.formValidate.endsdate,
          GiveLessonsTeacher:this.formValidate.GiveLessonsTeacher
        }

        axios.post("/api/AddteacherAndCourse",obj)
          .then((res) => this.getHomeInfoSucc(res));
      },

      getHomeInfoSucc(res) {

        console.log(res.data)

        if (res.data) {
          this.$Message.success("增加课程成功!");
          this.$router.push({ path: "/about/CourseList" });
        }

      }

    }
  };
</script>

<style scoped lang="scss">

    .Addthecourse {
        /*border: 1px solid red;*/
    }

    .main{
        margin-top: -30px;
    }

    .AddthecourseForm {
        max-width: 600px;
        margin: 0 auto;
        border-right: 1px solid #eee;
        padding: 0 15px;
    }


    .AddthecourseDate{
        display: flex;
        justify-content: space-evenly;
    }
</style>
