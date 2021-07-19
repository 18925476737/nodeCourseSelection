<template>
    <div class="home">



        <div class="LoginForm">
            <Divider>登录</Divider>

            <Form ref="formValidate" :model="formValidate" :rules="ruleValidate" :label-width="100">
                <FormItem label="用户名" prop="name">
                    <Input v-model="formValidate.name" placeholder="请输入你的用户名" autocomplete maxlength="5"
                           show-word-limit/>
                </FormItem>

                <FormItem label="密码" prop="password">
                    <Input v-model="formValidate.password" placeholder="请输入你的密码" autocomplete
                           icon="md-key"/>
                </FormItem>


                <FormItem label="身份" prop="identity">
                    <RadioGroup v-model="formValidate.identity">
                        <Radio label="Administrator">管理员</Radio>
                        <Radio label="teacher">教师</Radio>
                        <Radio label="student">学生</Radio>
                    </RadioGroup>
                </FormItem>

                <FormItem>
                    <Button type="primary" @click="handleSubmit('formValidate')">登录</Button>
                    <Button @click="handleReset('formValidate')" style="margin-left: 8px">重置</Button>
                </FormItem>
            </Form>

        </div>

    </div>
</template>

<script>
  // @ is an alias to /src

  import axios from "axios";


  export default {
    name: "home",
    inject:["showsome"],
    data() {
      return {
        formValidate: {
          name: "",
          password: "",
          identity: ""
        },
        ruleValidate: {
          name: [
            { required: true, message: "用户名不能为空", trigger: "blur" }
          ],
          password: [
            { required: true, message: "密码不能为空", trigger: "blur" }
          ],

          identity: [
            { required: true, message: "请选择身份", trigger: "change" }
          ]
        },
        name: ""
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

      getHomeInfo() {

        let obj = {
          name: this.formValidate.name,
          password: this.formValidate.password,
          identity: this.formValidate.identity
        };

        axios.post("/api/Getlogin", obj)
          .then((res) => this.getHomeInfoSucc(res));
      },

      getHomeInfoSucc(res) {

        if (res.data) {
          if (res.data.result === "error") {
            this.$Message.error("账号或密码错误!");

          }else if( res.data.result=== 'NotAdministrator'){
            this.$Message.error("你不是管理员，请重试！");
          }else {
            this.$Message.success("Success!");
            this.showsome()


            console.log(res.data)

            console.log(res.data.data1.identity)


            if(res.data.data1.identity ==="老师"){
              sessionStorage.setItem("username",this.formValidate.name)
              this.$router.push({ path: "/about/EditInfoTeacher" });
            }else if(res.data.data1.identity ==="管理员"){
              sessionStorage.setItem("username",this.formValidate.name)
              this.$router.push({ path: "/about/TeacherManagementList" });
            }else{
              sessionStorage.setItem("username",this.formValidate.name)
              this.$router.push({ path: "/about/EditInfostudent" });
            }

          }
        }
      }
    }
  };
</script>

<style lang="scss" scoped>
    .LoginForm {
      padding: 11px;
      border-left: 1px solid #eee;
      background: #a3a1a13b;
      margin: 0 auto;
      position: absolute;
      left: 50%;
      top: 45%;
      transform: translate(-50%, -50%);
    }
</style>
