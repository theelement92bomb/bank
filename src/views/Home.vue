<template>
  <center>
    <h1>Welcome to **** Bank</h1>
    <div class="container-fluid">
      <div class="row">
        <div class="col-lg-2"></div>
        <div class="col-lg-2">
          <button @click="display()">Check Balance</button>
        </div>
        <div class="col-lg-2">
          <button @click="cashDeposit()">Make a Cash Deposit</button><br />
          <div>Maximum deposit of $100</div>
        </div>
        <div class="col-lg-2">
          <button @click="deposit()">Make a Check Deposit</button>
        </div>
        <div class="col-lg-2">
          <button @click="withdraw()">Make a Withdraw</button>
        </div>
      </div>
      <div>Current Balance: {{ displayBalance }}</div>
      <div>
        <label for="toChange"></label>
        <input
          type="number"
          v-model="toChange"
          placeholder="Enter Deposit/Withdraw Here"
        />
      </div>
      <div>
        <label for="password">Password:</label><br />
        <input
          type="password"
          name="password"
          v-model="password"
          placeholder="Password"
        />
      </div>
      {{ passwordIncorrect }} {{ errorMsg }}
      <div>
        <button @click="changePassword()">Change Password</button>
      </div>
    </div>
    <div v-if="myModal">
      <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper">
            <div class="modal-dialog">
              <div class="modal-content">
                <div class="modal-header">
                  <h4 class="modal-title">Password Changer</h4>
                  <button type="button" class="close">
                    <span aria-hidden="true" @click="closeModal">&times;</span>
                  </button>
                </div>
                <div class="modal-body">
                  <label for="currentPassword">Current Password</label><br>
                  <input type="password" name="currentPassword" v-model="currentPassword"><br>
                  <label for="newPassword">New Password</label><br>
                  <input type="password" name="newPassword" v-model="newPassword"><br>
                  <label for="reconfirmPassword">Confirm New Password</label><br>
                  <input type="password" name="reconfirmPassword" v-model="reconfirmPassword"><br>
                  <button @click="confirmPasswordChange()">Submit</button><br>
                  {{passwordError}}
                </div>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </center>
</template>

<script>
export default {
  data() {
    return {
      currentBalance: 1000,
      password: "",
      toChange: "",
      displayBalance: "",
      passwordIncorrect: "",
      errorMsg: "",
      setPassword: "",
      myModal: false,
      currentPassword:"",
      newPassword:"",
      reconfirmPassword:"",
      passwordError:"",
    };
  },
  methods: {
    display() {
      if (this.checkPassword()) {
        this.displayBalance = this.currentBalance;
      }
    },
    checkPassword() {
      this.errorMsg = "";
      this.passwordIncorrect = "";
      if (this.password == this.setPassword) {
        return true;
      }
      this.passwordIncorrect = "Incorrect Password";
      return false;
    },
    cashDeposit() {
      if (this.checkPassword()) {
        if (this.toChange <= 100) {
          this.currentBalance += this.toChange;
        } else {
          this.errorMsg = "Deposit amount too large. Please try again.";
        }
      }
    },
    deposit() {
      if (this.checkPassword()) {
        this.currentBalance += this.toChange;
      }
    },
    withdraw() {
      if (this.checkPassword()) {
        if (this.toChange <= this.currentBalance) {
          this.currentBalance -= this.toChange;
        } else {
          this.errorMsg = "Insufficient amount remaining";
        }
      }
    },
    changePassword() {
      this.myModal = true;
    },
    closeModal(){
      this.myModal = false;
    },
    confirmPasswordChange(){
      if(this.currentPassword==this.setPassword){
        if(this.newPassword==this.reconfirmPassword){
          this.setPassword = this.newPassword
          this.passwordError = "Your new password has been set. Click the X to leave this box."
        }
        else{
          this.passwordError = "New passwords do not match"
        }
      }
      else{
        this.passwordError = "Current password does not match existing password"
      }
    }
  },
};
</script>

<style scoped></style>
