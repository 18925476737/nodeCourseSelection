<template>
    <div class="TeacherManagement">
        <div class="main">
            <div class="TeacherManagementForm">
                <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
                    <Divider>教师信息录入</Divider>
                    <FormItem label="用户名" prop="username">
                        <Input v-model="formValidate.username" placeholder="请输入用户名" show-word-limit
                               maxlength="8"/>
                    </FormItem>

                    <FormItem label="姓名" prop="name">
                        <Input v-model="formValidate.name" placeholder="请输入姓名 " show-word-limit
                               maxlength="4"/>
                    </FormItem>


                    <FormItem label="密码" prop="password">
                        <Input v-model="formValidate.password" placeholder="请输入密码" autocomplete
                               icon="md-key"/>
                    </FormItem>

                    <FormItem label="联系方式" prop="contactway">
                        <Input v-model="formValidate.contactway" placeholder="请输入联系方式"/>
                    </FormItem>

                    <FormItem label="邮箱" prop="mail">
                        <Input v-model="formValidate.mail" placeholder="请输入邮箱"/>
                    </FormItem>

                    <FormItem label="入职时间">
                        <div class="TeacherManagementDate">
                            <FormItem prop="date">
                                <DatePicker type="date" placeholder="选择时间"
                                            v-model="formValidate.date"></DatePicker>
                            </FormItem>
                            -
                            <FormItem prop="time">
                                <TimePicker type="time" placeholder="选择时间"
                                            v-model="formValidate.time"></TimePicker>
                            </FormItem>
                        </div>
                    </FormItem>


                    <FormItem label="描述" prop="desc">
                        <Input v-model="formValidate.desc" type="textarea" :autosize="{minRows: 2,maxRows: 5}"
                               placeholder="请输入描述备注"/>
                    </FormItem>

                    <FormItem>
                        <Button type="primary" @click="handleSubmit('formValidate')">提交</Button>
                        <Button @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>
                    </FormItem>
                </Form>
            </div>

        </div>
    </div>
</template>

<script>
  import axios from "axios";

  export default {
    name: "TeacherManagement",
    data() {
      return {
        formValidate: {
          username: "",
          name: "",
          password: "",
          contactway: "",
          mail: "",
          date: "",
          time: "",
          desc: ""
        },
        ruleValidate: {
          username: [
            { required: true, message: "用户名不能为空", trigger: "blur" }
          ],
          name: [
            { required: true, message: "姓名不能为空", trigger: "blur" }
          ],
          password: [
            { required: true, message: "密码不能为空", trigger: "blur" }
          ],
          mail: [
            { required: true, message: "邮箱不能为空", trigger: "blur" },
            { type: "email", message: "邮箱格式不正确", trigger: "blur" }
          ],
          date: [
            { required: true, type: "date", message: "请选择时间", trigger: "change" }
          ],
          time: [
            { required: true, type: "string", message: "请选择时间", trigger: "change" }
          ],
          desc: [
            { required: true, message: "请输入描述备注", trigger: "blur" },
            { type: "string", min: 20, message: "描述字数不够", trigger: "blur" }
          ]
        }
      };
    },
    methods: {
      handleSubmit(name) {
        this.$refs[name].validate((valid) => {
          if (valid) {
            this.getHomeInfo();


          } else {
            this.$Message.error("Fail!");
          }
        });
      },
      handleReset(name) {
        this.$refs[name].resetFields();
      },
      //
      getHomeInfo() {
        let obj = {
          username: this.formValidate.username,
          name: this.formValidate.name,
          password: this.formValidate.password,
          contactway: this.formValidate.contactway,
          mail: this.formValidate.mail,
          data: this.formValidate.date,
          desc: this.formValidate.desc
        };
        axios.post("/api/Addteacherdata", obj)
          .then((res) => this.getHomeInfoSucc(res));
      },
      getHomeInfoSucc(res) {
        if (res.data) {
          this.$Message.success("Success!");
          this.$router.push({ path: "/about/TeacherManagementList" });
        }
      }


    }
  };
</script>

<style scoped lang="scss">

    .TeacherManagement {
        /*border: 1px solid red;*/
    }

    .main {
        margin-top: -30px;
    }

    .TeacherManagementForm {
        max-width: 600px;
        margin: 0 auto;
        border-right: 1px solid #eee;
        padding: 0 15px;
    }


    .TeacherManagementDate {
        display: flex;
        justify-content: space-evenly;
    }
</style>
