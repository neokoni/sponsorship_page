<template>
  <!-- 主体方框部分 -->
  <div class="
        lg:h-[400px] lg:w-[900px] bg-[rgba(82,76,76,0.5)] backdrop-blur-xl 
        border-[rgba(181,181,181,1)] border lg:rounded-[20px]
        m-auto absolute top-0 left-0 right-0 bottom-0
        pt-[50px] pl-[50px] pr-[50px] pb-[50px]
        lg:flex items-center lg:justify-between
        text-[rgba(227,227,227,1)] text-base
        shadow-2xl

        w-4/5 h-5/6 rounded-[30px]
        ">
        <div class=" max-w-[350px] lg:h-full h-[38%]">
            <p class="text-3xl ">打钱</p>
            <p class=" pt-3">感谢你的赞助或向我付款(打钱)，请选择你喜欢的支付渠道以展开支付二维码，点击以展开详情</p>
        </div>
        <!-- 中间分隔线 -->
        <div class="lg:ml-6 lg:h-64 block border border-[rgba(153,151,151,1)] bg-[rgba(153,151,151,1)]"></div>
        <div class="lg:ml-6 lg:w-[300px]  text-center pt-4">请选择支付方式
            <!-- 使用v-for循环进行生成，在const payWay那里 -->
            <div v-for="i in payWay">
                <button class="border lg:h-16 lg:w-48 mt-4 rounded-[12px] border-[#C7BBBB] bg-[rgba(143,135,135,0.28)]
                     hover:bg-[rgba(151,147,147,0.71)] hover:text-white hover:shadow-xl
                      transition ease-in-out duration-500
                      h-14 w-40" @click="openQRcode(i.type)">
                    <img :src="i.logo" alt="">
                    <p v-text="i.name"></p>
                </button>
            </div>
        </div>
    </div>
    <!-- 覆盖部分 -->
    <div class="absolute left-0 top-0 bg-[rgba(41,41,41,0.5)] 
                h-screen w-screen 
                lg:text-base text-[rgba(196,196,196,1)]
                backdrop-blur-2xl hidden" id="qrcodepage">
        <!-- 使二维码部分居中 -->
        <div class="absolute left-1/2 lg:top-1/2 top-[45%] -translate-x-1/2 -translate-y-1/2">
            <p class=" text-center ">请使用<span id="payTypeTitle"></span>扫描二维码</p>
            <!-- 二维码主体部分 -->
            <div class="lg:h-[280px] lg:w-[280px] bg-[rgba(90,88,88,0.17)]
                        border border-[rgba(145,145,145,1)] rounded-2xl
                        m-auto mt-10
                        pt-[10px] pl-[10px] pr-[10px] pb-[10px]
                        h-[200px] w-[200px]">
                <img src="" alt="" id="qrcode" class="block">
            </div>
            <!-- 跳转按钮部分 -->
            <a id="openApp" class="hidden" target="_blank">
                <button class=" m-auto lg:w-[250px] lg:h-[70px] bg-[rgba(90,88,88,0.17)]
                                    border border-[rgba(145,145,145,1)] rounded-full
                                    mt-10 text-center block 
                                    hover:bg-[rgba(187,187,187,0.71)] hover:text-white hover:shadow-xl
                                    transition ease-in-out duration-500 
                                    h-14 w-40
                                    ">或直接跳转支付宝?</button>
            </a>
        </div>
        <!-- 关闭按钮处 -->
        <div class="w-10 h-10 bg-[rgba(107,107,107,0.2)] absolute 
                        lg:top-40 lg:right-48 lg:left-auto hover:shadow-xl 
                        border border-[rgba(145,145,145,1)] rounded-full
                        hover:bg-[rgba(59,59,59,0.2)] transition ease-in-out
                        left-1/2 top-[15%] -translate-x-1/2 -translate-y-1/2" @click="closeQRcode()">
            <img src="/icons/x.svg" alt="" class="m-auto pt-2 fill-white">
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    data() {
        return {
            // 支付方式
            payWay: [
                {
                    name: "支付宝",
                    logo: "",
                    type: "alipay"
                },
                {
                    name: "微信",
                    logo: "",
                    type: "wechat"
                },
                {
                    name: "QQ钱包",
                    logo: "",
                    type: "qq"
                }
            ]
        };
    },
    // @click用到的方法
    methods: {
        closeQRcode() {
            // 获取元素
            const qrcodePage = document.getElementById("qrcodepage");
            const qrcode = document.getElementById("qrcode");
            const payTypeTitle = document.getElementById("payTypeTitle")
            // 还原到初始值
            qrcode?.setAttribute("src", "");
            if (payTypeTitle) { payTypeTitle.textContent = ""; }
            qrcodePage?.classList.add("hidden");
            const openAPP = document.getElementById("openApp");
            if (openAPP) { openAPP.classList.add("hidden"); }
        },
        openQRcode(payPlatform: string) {
            // 获取元素
            const qrcodePage = document.getElementById("qrcodepage");
            const qrcode = document.getElementById("qrcode");
            const payTypeTitle = document.getElementById("payTypeTitle")
            const openApp = document.getElementById("openApp");
            // 判定选择的方式
            if (payPlatform == "alipay") {
                // 设置支付宝收款码
                qrcode?.setAttribute("src", "/qrcodes/alipay.png");
                // 设置显示的支付方式名称
                if (payTypeTitle) { payTypeTitle.textContent = "支付宝"; }
                // 对其显示跳转按钮
                if (openApp) { openApp.classList.remove("hidden"); }
                // 跳转的链接
                openApp?.setAttribute("href", "https://qr.alipay.com/fkx14150lufylbxvwp40u0a");
            }
            if (payPlatform == "wechat") {
                // 设置微信收款码
                qrcode?.setAttribute("src", "/qrcodes/wechat.png");
                // 设置显示的支付方式名称
                if (payTypeTitle) { payTypeTitle.textContent = "微信"; }
            }
            if (payPlatform == "qq") {
                // 设置QQ收款码
                qrcode?.setAttribute("src", "/qrcodes/qq.png");
                // 设置显示的支付方式名称
                if (payTypeTitle) { payTypeTitle.textContent = "QQ"; }
            }
            // 让显示
            qrcodePage?.classList.remove("hidden");
        },

    }
});
</script>