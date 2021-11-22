<template>
 <main>
       <div class="method__bank main-left">
           <h1 class="main-head">Verify Your Bank Details<br/> Now!</h1>
                        <div class="method__bank--1">
                            <select class="email-input" v-model="bankCode" name="Select Bank" id="Select-bank">
                                <option class="email-input" :value="''">Select Bank</option>
                                <option class="email-input" v-for="{ name, code } in bankList" :key="code" :value="code">{{ name }}</option>
                            </select>
                        </div>
                        <div class="method__bank--2">
                            <input class="email-input" type="text" v-model="bankAccount" name="Account Number" placeholder="Account Number" data-mask="0000000000">
                        </div>
                        <button class="main-start-btn" @click="onVerifyBtnClickHandle">
                            <span class="btn__text">VERIFY ACCOUNT NOW</span>
                        </button>
                        <div class="result">RESULT: {{ (verifyResponse.message ? verifyResponse.message : verifyResponse.accountName) || '' }}</div>
                        
   </div>
   <div class="main-img">
          <img src="../assets/bg-img.png" alt="img" />
      </div>
 </main>
</template>

<script>
export default {
    name: 'VerifyBankAccountMain',

    data() {
                return {
                    bankList: [],
                    bankCode: '',
                    bankAccount: '',
                    verifyResponse: {},
                }
            },
    mounted() {
        this.listAccount()
    },
    methods: {
        listAccount() {
            fetch('https://api.paystack.co/bank?perPage=2000&page=1')
                .then(response => response.json())
                .then(({ data }) => {
                    this.bankList = data;
                    });
        },
        onVerifyBtnClickHandle() {
            this.$alert("Verification completed");
            if(!this.bankAccount || !this.bankCode) return;
                fetch(`https://bank-verification-app.herokuapp.com/verify-account?bankCode=${this.bankCode}&accountNumber=${this.bankAccount}`)
                    .then(response => response.json())
                .then(({ data }) => {
                    this.verifyResponse = {
                        accountNumber: data.accountNumber,
                        accountName: data.accountName
                    };
                }).catch(({ message }) => this.verifyResponse = { message });
        }
    }
}
        
</script>

<style>
main {
        display: flex;
        flex-direction: row;
        width: 100%;
        margin-top: 80px;
    }
    .main-left {
        text-align: left;
        padding: 20px;
        margin-left: 140px;
        margin-top: 40px;
        color: rgb(20, 3, 94);
        width: 50%;
    }
    .main-head {
        font-size: 46px;
    }
    .main-text {
        font-weight: 100;
        width: 60%;
    }
     .cta {
        display: flex;
        flex-direction: row;
        margin-top: 40px;
    }
    .email-input {
        padding: 13px;
        outline: none;
        border: 1px solid #0066f55b;
        font-family: 'Poppins';
        font-size: 14px;
        border-radius: 8px;
        width: 60%;
        margin-right: 20px;
        margin-top: 30px;
    }
    .main-start-btn {
        background-color: #0066f5;
        padding: 13px;
        border: none;
        outline: none;
        color: #fff;
        font-family: Poppins;
        font-size: 14px;
        border-radius: 8px;
        font-weight: 400;
        cursor: pointer;
        width: 440px;
        margin-top: 30px;
    }
    .main-start-btn:hover {
        background: #055eda;
    }
    .result {
        margin-top: 20px;
        font-weight: 600;
    }
</style>