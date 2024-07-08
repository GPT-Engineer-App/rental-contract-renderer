# rental-contract-renderer

用繁中回答我的問題，問和我下面代碼中無法正常將出租人的姓名和身分證字號渲染在網頁上:

import dataStore from "@/stores/dataStore";
import { mapState, mapActions } from "pinia";

import contractInput from '../components/contractInput.vue';
import preview_btn from '../components/preview_btn.vue';
import send_btn from '../components/send_btn.vue';

export default {
    data() {
        return {
            tenant_identity: "",  
            tenant_name: "",
            tenant_home_address: "",
            tenant_contact_address: "",
            tenant_phone: "",
            tenant_email: "",
            owner_name: "",  //從註冊資訊抓
            owner_identity: "", //從註冊資訊抓
            owner_home_address: "", //從註冊資訊抓
            owner_contact_address: "", //從註冊資訊抓
            owner_phone: "",  //從註冊資訊抓
            start_date: "",
            end_date: "",
            c_other: "",
            sign_date: "",
            cut_reason: "",
            cut_date: "",
            ai:"",
        // 房間資訊抓
            address: "",
            floor: "",
            r_id: "",
            rent_p: "",
            deposit: "",
            cut_p: "",
            eletric_p: "",
            water_p: "",
            manage_p: "",
            acreage: "",
            parking: false,
            equip: "",
            r_other: "",

            //////////////////
            loginObj: { // 添加 loginObj 並初始化為空物件
                ownerIdentity: ""
            },
    }
},
    computed: {
       //使用pinia中房間站存資訊
       ...mapState(dataStore, ['roomObj','registerObj'])
    },
    created() {
        // this.getRegisterData();
        //this.getRoomData();
         // 從路由參數中獲取房間資訊
        this.roomInfo = this.$route.params.roomInfo;
    },
    mounted() {
       
    },
    methods: {
      
        // async getRegisterData() {
        // try {
        //     const response = await fetch('http://localhost:8080/rent/account');
        //     const data = await response.json();
        //     // 假設 API 返回的資料是這樣的結構: { owner_name: '...', owner_phone: '...', owner_email: '...' }
        //     this.owner_name = data.owner_name;
        //     this.owner_phone = data.owner_phone;
        //     this.owner_email = data.owner_email;
        // } catch (error) {
        //     console.error('Error fetching register data:', error);
        // }
        // },
        // async getRoomData() {
        // try {
        //     const response = await fetch('http://localhost:8080/room/creatRoom1');
        //     const data = await response.json();
        //     // 假設 API 返回的資料是這樣的結構: { r_id: '...', address: '...', rent_p: '...' }
        //     this.r_id = data.r_id;
        //     this.address = data.address;
        //     this.rent_p = data.rent_p;
        // } catch (error) {
        //     console.error('Error fetching room data:', error);
        // }
        // },
        addContractToDB() {
        let testObj = {
            tenantIdentity: this.tenant_identity,  
            tenantName: this.tenant_name,
            tenantHomeAddress: this.tenant_home_address,
            tenantContactAddress: this.tenant_contact_address,
            tenantPhone: this.tenant_phone,
            tenantEmail: this.tenant_email,
            ownerName: this.registerObj.ownerName, //從註冊資訊抓
            ownerIdentity: this.registerObj.ownerIdentity, //從註冊資訊抓
            ownerHomeAddress: this.owner_home_address, 
            ownerContactAddress: this.owner_contact_address, 
            //ownerPhone: this.registerObj.owner_phone, //契約的表沒有這個欄位，要從註冊抓，但我建議再SQL新增這個欄位，因為房東可能想註冊的電話跟連絡他的電話不一樣
            startDate: this.start_date,
            endDate: this.end_date,
            cOther: this.c_other,
            signDate: this.sign_date,
            cutReason: this.cut_reason,
            cutDate: this.cut_date,
            signDate: this.sign_date,
            ai:this.ai,
            //從房間抓
            address: this.roomObj.address,
            floor: this.roomObj.floor,
            roomId: this.roomObj.roomId,
            rentP: this.roomObj.rentP,
            deposit: this.roomObj.deposit,
            cutP: this.roomObj.cutP,
            eletricP: this.roomObj.eletricP,
            waterP: this.roomObj.waterP,
            manageP: this.roomObj.manageP,
            acreage: this.roomObj.acreage,
            parking: this.roomObj.parking,
            equip: this.roomObj.equip,
            rOther: this.roomObj.rOther,
        
        };
        fetch("http://localhost:8080/contract/createContract", {
            method: "POST",
            headers: {
            "Content-Type": "application/json"
            },
            body: JSON.stringify(testObj)
        })
        .then(res => res.json())
        .then(data => {
            console.log(data);
        });
        }
    },
    components: {
        contractInput,
        preview_btn,
        send_btn,
    },
};
</script>

<template>
    <div class="areaMom">
        <h1>新增契約書</h1>
        <div class="area">
            <div class="bigArea">
                
                <br>
                <div class="roomInfo">
                    <h2>租賃物件資訊</h2>
                    <br>
                    <div class="rent_time">
                    <label for="start_time">租賃期間 自：</label>
                    <input type="date" id="start" style="font-size: 22px;" min="1970-01-01" max="2050-12-31" v-model="start_date"/>
                    <label for="end_time">到：</label>
                    <input type="date" id="end" style="font-size: 22px;" min="1970-01-01" max="2050-12-31" v-model="end_date" />
                    </div>
                    <br>
                    租賃物件地址: <p>{{ roomObj.address }}</p>
                    <br>
                    樓層:<p>{{ roomObj.floor }}</p>
                    <br>
                    <!-- 這邊的rId是小徐的，創建完後才會變成我的roomId -->
                    房號:<p>{{ roomObj.roomId }}</p>
                    <br>
                    租金/月:<p>{{roomObj.rentP }} </p>
                    <br>
                    押金: <p>{{ roomObj.deposit }}</p>
                    <br>
                    管理費/月:<p>{{roomObj.manageP}}</p>
                    <br>
                    電費/度: <p>{{ roomObj.eletricP }}</p>
                    <br>
                    水費/月:<p>{{ roomObj.waterP }} </p>
                    <br>
                    面積: <p>{{ roomObj.acreage }}</p>
                    <br>
                    車位:<p>{{roomObj.parking}}</p>
                    <br>
                    設備:<p>{{ roomObj.equip }}</p>
                    <br>
                    物件備註:<p>{{ roomObj.rOther }}</p>
                    
                    </div>
                </div>

                <div class="line">
                
                </div>
            <div class="bigArea2">
                <h2>立契約書人</h2>
                <div class="Info">
                    <br>
                    <h4>出租人姓名:</h4> <p>{{ registerObj.ownerName }}</p>
                    <br>
                    身分證字號: <p> {{registerObj.ownerIdentity}}</p>
                    <br>
                    戶籍地址(營業登記地址): <input type="text" v-model="owner_home_address" class="input-box">
                    <br>
                    通訊地址: <input type="text" v-model="owner_contact_address" class="input-box">
                    <br>
                    <!-- 這邊房東電話不建議寫死，因為註冊電話應該可以和連絡電話不一樣，，建議契約的SQL表另外新增owner_phone欄位而不是直接引用註冊的電話 -->
                    連絡電話:{{ registerObj.ownerPhone }}
                    <br>
                    <br>
                    <h4>承租人姓名:</h4> <input type="text" v-model="tenant_name" class="input-box" style="font-weight: bold; font-size: 20px;">
                    <br>
                    身分證字號: <input type="text" v-model="tenant_identity" class="input-box">
                    <br>
                    戶籍地址(營業登記地址): <input type="text" v-model="tenant_home_address" class="input-box">
                    <br>
                    通訊地址: <input type="text" v-model="tenant_contact_address" class="input-box">
                    <br>
                    email: <input type="text" v-model="tenant_email" class="input-box">
                    <br>
                    連絡電話: <input type="text" v-model="tenant_phone" class="input-box">
                </div>
                <br>
                <h3>契約中止</h3>
                <div class="cut">
                    <br>
                    中止原因: <input type="text" v-model="cut_reason" class="input-box">
                    <br>
                    違約金: <input type="text" v-model="cut_p" class="input-box">
                    <br>
                    中止日期: <input type="text" v-model="cut_date" class="input-box">
                </div>
                <br>
                <h3>其他備註(或個別磋商條款)</h3>
                <div class="input-wrapper">
                    <textarea class="input-box" type="textarea" placeholder="Enter your text" v-model="c_other"></textarea>
                    <span class="underline"></span>
                </div>
                <br>
                <h3>立約日期:</h3>
                <input type="date" id="start" style="font-size: 22px;" min="1970-01-01" max="2050-12-31" v-model="start_date"/>
            
                <div class="btn"> 
                
                    <send_btn class="space-between" @click="addContractToDB"/>
                </div>
            </div>
        </div>
    </div>

</template>

## Collaborate with GPT Engineer

This is a [gptengineer.app](https://gptengineer.app)-synced repository 🌟🤖

Changes made via gptengineer.app will be committed to this repo.

If you clone this repo and push changes, you will have them reflected in the GPT Engineer UI.

## Tech stack

This project is built with .

- Vite
- React
- shadcn-ui
- Tailwind CSS

## Setup

```sh
git clone https://github.com/GPT-Engineer-App/rental-contract-renderer.git
cd rental-contract-renderer
npm i
```

```sh
npm run dev
```

This will run a dev server with auto reloading and an instant preview.

## Requirements

- Node.js & npm - [install with nvm](https://github.com/nvm-sh/nvm#installing-and-updating)
