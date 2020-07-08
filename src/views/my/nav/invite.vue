<template>
    <div class="page">
        <m-header title="分享推荐" :canback="Boolean(1)" :nobg="true"></m-header>
        <section class="body">
            <div class="img-box">
                <img class="img" :src="inviteSrc" alt="">
                <p class="tip">扫描二维码即可分享好友哟</p>
                <p v-if="userInfo" class="url">{{inviteUrl}}</p>
                <input class="inp_url" type="text" id="url" :value="inviteUrl" readonly>
                <button ref="copy" class="btn btn-copy" data-clipboard-target="#url" data-clipboard-action="copy" @click="copy">复制</button>
            </div>
        </section>
    </div>
</template>
<script>
import QRCode from 'qrcode';
import {mapGetters} from 'vuex';
import Clipboard from 'clipboard'
export default {
    name:"invite",
    data(){
        return {
            inviteSrc:null,
            inviteUrl:null,
            myClipboard:null
        }
    },
    mounted(){
        // console.log(this.userInfo)
        this.initData();
        this.setCode(this.inviteUrl);
        this.myClipboard = new Clipboard(this.$refs['copy']);
    },
    computed:{
        ...mapGetters(['uid','userInfo','api',])
    },
    methods:{
        initData(){
            this.inviteUrl = this.api+'/dist/index.html#'+'/register/'+this.uid+'/'+this.userInfo.mobile;
        },
        setCode(url){
            QRCode.toDataURL(url).then(url => {
                this.inviteSrc = url;
            })
            .catch(err => {
                console.error(err)
            })
        },
        copy(){
            this.myClipboard.on('success',(e)=>{
                mui.toast('复制成功！');
            })
            this.myClipboard.on('error',(e)=>{
                mui.toast('复制失败，请手动复制');
            })
        }
    }
}
</script>
<style lang="less" scoped>
@import '~link-less';
    .page{
        background-image: url(../../../assets/images/gzlz/invate_bg.png);
        background-size: 100% 100%;
    }
    .body {
        background: transparent;background-color: transparent;
        text-align: center;
        border-top: 1px solid rgba(255,255,255,0.3);
        .title {
            font-size: 40px;
            color: @primary-color;
            margin-top: 5%;
        }
        .url {
            // margin-top: 75%;
            padding: 0 20px; 
            margin-bottom: 20px;color: #333;
            word-break: break-all;
            user-select: text;user-select:text;
        }
        .inp_url {
            position: absolute;
            top: 0;left: 0;opacity: 0;
        }
        .img-box {
            position: absolute;top: 20%;left: 50%;
            height: 650px;width: 75%;
            background-color: white;border-radius: 10px;
            padding: 50px 0;
            .img {
                width: 360px;
            }
            .transform(translateX(-50%));
            .tip {
                color: @primary-color;
                // margin-top: 77px;
            }
            .btn-copy {
                background-color: @btn-primary-color;
                font-size: 28px;
            }
        }
        @media screen and (max-height:667px){
            .img-box {
                position: absolute;
                bottom: 10%;
                left: 50%;
                .transform(translateX(-50%));
            }
        }
    }
</style>
