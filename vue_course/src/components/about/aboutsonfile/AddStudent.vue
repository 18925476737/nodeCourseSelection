<template>
    <div class="AddStudent">
        <div class="main">
            <div class="AddStudentForm">
                <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="80">
                    <Divider>学生信息录入</Divider>

                    <FormItem label="学生姓名" prop="NameOfstudent">
                        <Input v-model="formValidate.NameOfstudent" placeholder="请输入学生姓名"
                               show-word-limit maxlength="4"/>
                    </FormItem>

                    <FormItem label="学生密码" prop="PwdOfstudent">
                        <Input v-model="formValidate.PwdOfstudent" placeholder="请输入学生密码 "
                               show-word-limit maxlength="10"/>
                    </FormItem>


                    <FormItem label="联系方式" prop="ContactMethod">
                        <Input v-model="formValidate.ContactMethod" placeholder="请输入学生联系方式"
                               show-word-limit maxlength="11"/>
                    </FormItem>


                    <FormItem label="邮箱" prop="mail">
                        <Input v-model="formValidate.mail" placeholder="请输入学生邮箱"/>
                    </FormItem>

                    <FormItem label="Gender" prop="gender">
                        <div class="Addgender">

                            <RadioGroup v-model="formValidate.gender">
                                <Radio label="男">男</Radio>
                                <Radio label="女">女</Radio>
                            </RadioGroup>
                        </div>

                    </FormItem>

                    <FormItem label="出生日期">
                        <div class="AddStudentDate">
                            <FormItem prop="date">
                                <DatePicker type="date" placeholder="请选择日期"
                                            v-model="formValidate.DateOfBirth"></DatePicker>
                            </FormItem>
                        </div>
                    </FormItem>


                    <FormItem label="班级" prop="ClassAndGrade">
                        <div class="ClassAndGrademian">
                            <Select v-model="formValidate.ClassAndGrade" placeholder="请选择班级"
                                    class="Selectinput">
                                <Option value="软件1班">软件1班</Option>
                                <Option value="软件2班">软件2班</Option>
                                <Option value="大数据1班">大数据1班</Option>
                                <Option value="大数据2班">大数据2班</Option>
                            </Select>
                        </div>
                    </FormItem>


                    <FormItem label="描述" prop="desc">
                        <Input v-model="formValidate.desc" type="textarea" :autosize="{minRows: 2,maxRows: 5}"
                               placeholder="请输入备注"/>
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
    name: "AddStudent",
    data() {
      return {
        formValidate: {
          NameOfstudent: "",
          PwdOfstudent: "",
          ContactMethod: "",
          DateOfBirth: "",
          gender: "",
          ClassAndGrade: "",
          desc: "",
          mail: ""
        },
        ruleValidate: {
          NameOfstudent: [
            { required: true, message: "姓名不能为空", trigger: "blur" }
          ],
          PwdOfstudent: [
            { required: true, message: "密码不能为空", trigger: "blur" }
          ],
          ContactMethod: [
            { required: true, message: "联系电话不能为空", trigger: "blur" }
          ],
          mail: [
            { type: "email", message: "邮箱格式错误", trigger: "blur" }
          ],
          DateOfBirth: [
            { required: true, type: "date", message: "请选择出生日期", trigger: "change" }
          ],
          gender: [
            { required: true, message: "请选择性别", trigger: "change" }
          ],

          ClassAndGrade: [
            { required: true, message: "班级不能为空", trigger: "blur" }
          ],
          desc: [
            { type: "string", min: 10, message: "备注字数太短了", trigger: "blur" }
          ]
        }
      };
    },
    methods: {
      handleSubmit(PwdOfstudent) {
        this.$refs[PwdOfstudent].validate((valid) => {
          if (valid) {

            this.getHomeInfo();

          } else {
            this.$Message.error("Fail!");
          }
        });
      },
      handleReset(PwdOfstudent) {
        this.$refs[PwdOfstudent].resetFields();
      },

      getHomeInfo() {
        let obj = {
          NameOfstudent: this.formValidate.NameOfstudent,
          PwdOfstudent: this.formValidate.PwdOfstudent,
          ContactMethod: this.formValidate.ContactMethod,
          contactway: this.formValidate.contactway,
          DateOfBirth: this.formValidate.DateOfBirth,
          gender: this.formValidate.gender,
          ClassAndGrade: this.formValidate.ClassAndGrade,
          desc: this.formValidate.desc,
          mail: this.formValidate.mail
        };
        axios.post("/api/AddStudentAndClass", obj)
          .then((res) => this.getHomeInfoSucc(res));
      },
      getHomeInfoSucc(res) {


        if (res.data) {
          this.$Message.success("成功!");
          this.$router.push({ path: "/about/StudentList" });
        }

      }
    }
  };
</script>

<style scoped lang="scss">

    .AddStudent {
        margin-top: -60px;
    }

    .main {
        margin-top: -30px;
    }

    .AddStudentForm {
        max-width: 600px;
        margin: 0 auto;
        border-right: 1px solid #eee;
        padding: 0 15px;
    }


    .AddStudentDate {
        padding-left: 13px;
        display: flex;
        justify-content: space-between;
    }

    .Addgender {
        text-align: left;
        padding-left: 15px;
    }

    .Selectinput {
        width: 200px;
    }

    .ClassAndGrademian {
        text-align: left;
        padding-left: 13px;
    }
</style>
