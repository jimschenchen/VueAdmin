<template>
    <div>
        <div class="bg"></div>
        <div class="container">
            <div class="columns">
                <div class="column col-12 flower"></div>
                <div class="column col-12">
                    <div class="btn-group btn-group-block">
                        <button class="btn" :class="{active: item.current}" v-for="item in menuTab" :key="item.id" @click="toggleMenu(item)">{{item.txt}}</button>
                    </div> 
                </div>
                <div class="column col-12">
                    <form v-if="menuTab[0].current">
                        <div class="form-group" :class="this.form.mail.error">
                            <label class="form-label">邮箱</label>
                            <input class="form-input" type="text" placeholder="邮箱" v-model="form.mail.content">
                            <p class="form-input-hint">{{form.mail.hint}}</p>
                        </div>
                        <div class="form-group" :class="this.form.pass.error">
                            <label class="form-label">密码</label>
                            <input class="form-input" type="password" placeholder="密码" v-model="form.pass.content">
                            <p class="form-input-hint">{{form.pass.hint}}</p>
                        </div>
                        <button class="btn btn-primary" @click="submit">登录</button>
                    </form>

                    <form v-if="menuTab[1].current">
                        <div class="form-group" :class="this.form.mail.error">
                            <label class="form-label">邮箱</label>
                            <input class="form-input" type="text" placeholder="邮箱" v-model="form.mail.content">
                            <p class="form-input-hint">{{form.mail.hint}}</p>
                        </div>
                        <div class="form-group" :class="this.form.pass.error">
                            <label class="form-label">密码</label>
                            <input class="form-input" type="password" placeholder="密码" v-model="form.pass.content">
                            <p class="form-input-hint">{{form.pass.hint}}</p>
                        </div>
                        <div class="form-group" :class="this.form.pass2.error">
                            <label class="form-label">再次输入密码</label>
                            <input class="form-input" type="password" placeholder="密码" v-model="form.pass2.content">
                            <p class="form-input-hint">{{form.pass2.hint}}</p>
                        </div>
                        <button class="btn btn-primary" @click="submit">登录</button>
                    </form>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
import { Login } from "@/api/login"
export default {
    name: "login",
    data() {
        return {
            menuTab: [
                { txt: "登录", current: true},
                { txt: "注册", current: false},
            ], 
            form: {
                mail: {content: '', error: '', hint: ''},
                pass: {content: '', error: '', hint: ''},
                pass2: {content: '', error: '', hint: ''}
            }
        }  
    },
    methods: {
        toggleMenu(data) {
            this.menuTab.forEach(element => {
                element.current = false
            });
            data.current = true;
        },
        submit() {
            // 定义判定变量
            let isSuccs = 0;

            // valide mail
            let reg = /^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/;
            if (!this.form.mail.content) {
                this.form.mail.hint = '邮箱不能为空!';
                this.form.mail.error = 'has-error';
                isSuccs = -1;
            } else if (!reg.test(this.form.mail.content)) {
                this.form.mail.hint = '请输入正确的邮箱!';
                this.form.mail.error = 'has-error';
                isSuccs = -1;
            } else {
                this.form.mail.hint = '邮箱正确！';
                this.form.mail.error = 'has-success';
            }

            // valide pass
            let reg2 = /^(?!\D+$)(?![^a-zA-Z]+$)\S{8,20}$/;
            if (!this.form.pass.content) {
                this.form.pass.hint = '密码不能为空!';
                this.form.pass.error = 'has-error';
                isSuccs = -1;
            } else if (!reg2.test(this.form.pass.content)) {
                this.form.pass.hint = '请输入8-20位数字+字母的密码！';
                this.form.pass.error = 'has-error';
                isSuccs = -1;
            } else {
                this.form.pass.hint = '密码正确！';
                this.form.pass.error = 'has-success';
            }

            // 注册专用
            if (this.menuTab[1].current) {
                // valide pass2
                if (this.form.pass2.content != this.form.pass.content) {
                    this.form.pass2.hint = '密码不一致!';
                    this.form.pass2.error = 'has-error';
                    isSuccs = -1;
                } else {
                    this.form.pass2.hint = '密码一致！';
                    this.form.pass2.error = 'has-success';
                }
            }

            if (isSuccs == 0) {
                // 判定成功
                Login()
            }
        }
    }
}
</script>


<style lang="scss" scoped>

.bg{
    position: fixed;
    top: 0px;
    left: 0px;
    height: 100%;
    width: 100%;
    z-index: -99999;
}

.login-wrap {
    width: 330px;
    margin: auto;
}

.menu-tab {
    text-align: center;
    li {
        display: inline-block;
        width: 88px;
        line-height: 36px;
        font-size: 14px;
        // color: #fff;
        border-radius: 2px;
        cursor: pointer;
    }
}

.current {
    background-color: rgba(0, 0, 0, .1)
}

.btn {
    width: 100%;
}

.flower{
    height: 250px;
    width: 100%;
    background: url("../../assets/flower.jpg");
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center;
    background-origin: border-box;
}
</style>