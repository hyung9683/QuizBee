<template>
    <main class="mt-5">
        <div class="login-form">
            <div class="logo">
                <img src='../assets/logo.png'>
            </div>
            <div>
                <label for="user_id" class="me-2">아이디 :</label>
                <input type="id" @keyup.enter="login()" class="form-control" placeholder="아이디를 입력해주세요" v-model="user_id" />
                <label for="floatingInput"></label>
            </div>

            <div>
                <label for="user_id" class="me-3">비밀번호 :</label>
                <input type="password" @keyup.enter="login()" class="form-control2" placeholder="8~12자 내외, 특수문자 포함해 비밀번호를 입력" v-model="user_passwd" />
                <label for="floatingPassword"></label>
            </div>

            <div>
                <button type="button" class="btn" @click="login">로그인</button>
                <img src="../assets/kakaoLogin.png" class="btn_kakao" @click="kakaoLogin">
            </div>
<div></div>
            <div class="find" style="margin-left:25%;" @click="goToFind">아이디 / 비밀번호 찾기 </div>
             <div class="find" style="margin-top: -8.8%; margin-left: 58%;" @click="goToJoin"> | 회원가입 </div>
        </div>
    </main>
</template>
    
<script>
import axios from 'axios';

export default {
    data() {
        return {
            user_id: '',
            user_passwd: '',
        };
    },
    computed: {
        user() {
            return this.$store.state.user; 
        },
    },
   mounted() {
        
        
    },
    methods: {
        login() {
            axios({
                url: "http://localhost:3000/auth/login_process",
                method: "POST",
                data: {
                    user_id: this.user_id,
                    user_passwd: this.user_passwd
                },
            })
                .then(res => {
                    if (res.data.message == 'undefined_id') {
                        this.$swal("존재하지 않는 아이디입니다.")
                    }
                    else if (res.data.message == 'incorrect_pw') {
                        this.$swal("비밀번호가 틀렸습니다.")
                    }
                    else {
                        this.$store.commit("user", { user_id: this.user_id, user_no: res.data.message })
                        this.$swal({
                            position: 'top',
                            icon: 'success',
                            title: '로그인 성공!',
                            showConfirmButton: false,
                            timer: 1000
                        })
                    }
                        this.$router.push({ path: '/' });  // 메인 화면으로 이동
                })
                .catch(err => {
                    console.log({err, message:'에러'});
                })
        },

        goToFind() {
            this.$router.push({ path: 'find' });
        },
        goToJoin() {
            this.$router.push({ path: 'join' });
        },
        //=====================================================카카오==========================================//0
        kakaoLogin() {

            window.Kakao.Auth.login({
                scope: "profile_nickname, account_email",
                success: this.getKakaoAccount,
            });
        },
        getKakaoAccount() {
            window.Kakao.API.request({
                url: "/v2/user/me",
                success: (res) => {
                    const kakao_account = res.kakao_account;
                    const email = kakao_account.email; //카카오 이메일
                    const nickname = kakao_account.profile.nickname;
                    // this.user_id = email

                    axios({
                        url: "http://localhost:3000/auth/kakaoLoginProcess",
                        method: "POST",
                        data: {
                            user_id: email,
                            user_nick: nickname
                        },
                    }).then(res => {
                        if (res.data.message == '저장완료') {

                            this.$swal({
                                position: 'top',
                                icon: 'success',
                                title: '회원가입 성공!',
                                showConfirmButton: false,
                                timer: 1000
                            })

                        }
                        else {
                            this.$store.commit("user", { user_id: email, user_no: res.data.message })
                            this.$swal({
                                position: 'top',
                                icon: 'success',
                                title: '로그인 성공!',
                                showConfirmButton: false,
                                timer: 1000


                            }).then(() => {
                                window.location.href = "http://localhost:8080";
                            })

                        }
                    })
                        .catch(err => {
                            console.log(err);
                        })


                },
                fail: (error) => {
                    // this.$router.push("/errorPage");
                    console.log(error);
                },
            });

        },



    },
};
</script>

<style scoped>
@font-face {
    font-family: 'GmarketSansMedium';
    src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_2001@1.1/GmarketSansMedium.woff') format('woff');
    font-weight: normal;
    font-style: normal;
}


* {
    padding: 0;
    margin: 0;

}

.logo img {
    width: auto;
    height: auto;
    max-width: 300px;
    display: block;
    margin: auto;
    position: relative;
    top: 40px;
    margin-left: 85px;
}

.login-form {
    display: grid;
    width: 450px;
    height: 600px;
    margin: 6% auto;
    border: solid 2px rgb(237, 237, 237);
    background-color: #fcf9db;
    background: linear-gradient(180deg, rgb(253, 238, 204), rgb(253, 245, 221));
    box-shadow: 0px 1px 30px 2px rgb(238, 238, 238);
    border-radius: 30px;
}

.me-2{
    height: 48px;
    width: 10px;
    font-size: 16px;
    display: inline;
    margin-left: 4%;
    margin-bottom: 10px;
    text-align: center;
    border-radius: 8px;
    position: relative;
    top: 70px;
    font-family: 'GmarketSansMedium';
}
.me-3 {
    height: 48px;
    width: 10px;
    font-size: 16px;
    display: inline;
    margin-left: 4%;
    margin-bottom: 10px;
    text-align: center;
    border-radius: 8px;
    position: relative;
    top: 70px;
    font-family: 'GmarketSansMedium';
}

.login-form .form-control {
    height: 48px;
    width: 75%;
    font-size: 16px;
    display: inline;
    margin-left: 10px;
    margin-bottom: 10px;
    border: solid 2px rgb(237, 237, 237);
    text-align: center;
    border-radius: 8px;
    position: relative;
    top: 70px;
    font-family: 'GmarketSansMedium';
}

.login-form .form-control2 {
    height: 48px;
    width: 75%;
    font-size: 16px;
    display: inline;
    margin-left: -11px;
    margin-bottom: 10px;
    border: solid 2px rgb(237, 237, 237);
    text-align: center;
    border-radius: 8px;
    position: relative;
    top: 70px;
    font-family: 'GmarketSansMedium';
}

input::placeholder {
    color: #aaa;
}

input:focus {
    outline: 2px solid #ffc905;
}

.login-form .btn {
    height: 40%;
    width: 100%;
    font-size: 16px;
    display: inline;
    margin-left: 0%;
    margin-bottom: 50px;
    border: solid 2px rgb(255, 204, 122);
    border-radius: 8px;
    background-color: rgb(255, 210, 107);
    position: relative;
    top: 90px;
    font-family: 'GmarketSansMedium';
}

.login-form .btn:hover {
    cursor: pointer;
}

/* sns btn */
.login-form .btn_kakao {
    /* scale: 45%; */
    width: 100.3%;
    height: 35%;
    position: relative;
    margin-left: -0.3%;
    top: 30px;


}


.login-form .btn_kakao:hover {
    cursor: pointer;
}


.find {
    position: relative;
    height: 30px;
    font-size: 0.8rem;
    color: #aaa;
    cursor: pointer;
    top:65%;
}
</style>