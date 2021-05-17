<template>
  <div
    class="subscribe gradiant-orange d-flex d-flex-column align-center justify-center mb-50"
  >
    <strong
      ><div class="title mt-20 mb-20">
        SIGN UP FOR UPDATES
      </div></strong
    >
    <form
      @submit.prevent="submit"
      ref="form"
      class="d-flex d-flex-column align-center"
    >
      <div class="d-flex align-center">
        <label for="name" class="lable">Name:</label>
        <div>
          <input
            :class="`${isNameValid.error ? 'error-input' : ''}`"
            @focus="isNameValid.focus = true"
            @blur="isNameValid.blur = true"
            ref="name"
            id="name"
            placeholder="John Smith"
            class="input"
            type="text"
            v-model.trim="name"
          />
          <div class="error-message">{{ isNameValid.message }}</div>
        </div>
      </div>
      <div class="d-flex align-center">
        <label for="email" class="lable">E-mail:</label>
        <div>
          <input
            :class="`${isEmailValid.error ? 'error-input' : ''}`"
            @blur="isEmailValid.blur = true"
            ref="email"
            id="email"
            placeholder="j.smith@gmail.com"
            class="input"
            type="email"
            v-model.trim="email"
          />
          <div class="error-message">{{ isEmailValid.message }}</div>
        </div>
      </div>
      <btn ref="submitButton" classes="blue mt-10 mb-10" subscribe />
    </form>
    <div class="mt-10">{{ message }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      email: "",
      message: "",
      isNameValid: {
        message: "",
        error: false,
        blur: false,
      },
      isEmailValid: {
        message: "",
        error: false,
        blur: false,
      },
    };
  },
  methods: {
    async submit() {
      this.applyNameValidation();
      this.applyEmailValidation();

      if (this.isNameValid.error || this.isEmailValid.error) {
        return;
      }
      // check if there is a subscriber with the same email address already. If so, return!
      const checkResult = await this.checkingForExistingSubscribers();
      if (checkResult.data === "Exist") {
        this.message = "You're Already Subscribed!";
        setTimeout(() => {
          this.message = "";
        }, 5000);
        this.resetForm();
        return;
      }
      // send the new subscriber's data to the firestore
      await this.sendToFirebase();
      // send an email to me
      this.sendingEmailToMe();
      this.message = "Thank you for Subscribing :-)";
      setTimeout(() => {
        this.message = "";
      }, 5000);
      //restart the form
      this.resetForm();
    },
    applyNameValidation(reset = false) {
      if (this.name.trim() === "" && !reset) {
        this.isNameValid.message = "The name field is required!";
        this.isNameValid.error = true;
      } else {
        this.isNameValid.message = "";
        this.isNameValid.blur = false;
        this.isNameValid.error = false;
      }
    },
    applyEmailValidation(reset = false) {
      if (this.email.trim() === "" && !reset) {
        this.isEmailValid.message = "The email field is required!";
        this.isEmailValid.error = true;
      } else if (
        !/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(this.email.trim()) &&
        !reset
      ) {
        this.isEmailValid.message = "Please enter a valid Email Address!";
        this.isEmailValid.error = true;
      } else {
        this.isEmailValid.message = "";
        this.isEmailValid.blur = false;
        this.isEmailValid.error = false;
      }
    },
    resetForm() {
      this.name = "";
      this.email = "";
      this.$nextTick(() => {
        this.$refs.name.blur();
        this.$refs.email.blur();
        this.applyNameValidation(true);
        this.applyEmailValidation(true);
      });
    },
    checkingForExistingSubscribers() {
      return new Promise((resolve, reject) => {
        firestore
          .collection("subscribers")
          .where("email", "==", this.email.toLowerCase())
          .get()
          .then((result) => {
            if (!result.empty) {
              resolve({ data: "Exist" });
            } else {
              resolve({ data: "NotExist" });
            }
          });
      }).catch(function(error) {
        reject("Error");
        console.log("Error getting documents: ", error);
      });
    },
    sendToFirebase() {
      return new Promise((resolve, reject) => {
        firestore
          .collection("subscribers")
          .add({
            name: this.name,
            email: this.email.toLowerCase(),
          })
          .then(() => {
            resolve("Done");
          })
          .catch((error) => {
            reject("Error");
            console.error("Error adding document in the database: ", error);
          });
      });
    },
    sendingEmailToMe() {
      return new Promise((resolve, reject) => {
        const sendSubscriberEmail = functions.httpsCallable(
          "sendSubscriberEmail"
        );
        sendSubscriberEmail({
          name: this.name,
          email: this.email.toLowerCase(),
        });
        resolve("Email has been sent!");
      });
    },
  },
  watch: {
    name() {
      this.applyNameValidation();
    },
    email() {
      this.applyEmailValidation();
    },
    "isNameValid.blur": {
      handler() {
        this.applyNameValidation();
      },
    },
    "isEmailValid.blur": {
      handler() {
        this.applyEmailValidation();
      },
    },
  },
};
</script>

<style scoped>
.subscribe {
  height: 350px;
  box-shadow: 10px 10px #d8d7c1;
  border: 1.5px solid #fff !important;
  border-radius: 8px;
}

.title {
  font-size: 30px;
  line-height: 35px;
  text-align: center;
}

@media (max-width: 470px) {
  .title {
    font-size: 24px;
    line-height: 30px;
  }
}

.gradiant-orange {
  background: rgb(241, 145, 139);
  background: -moz-linear-gradient(
    90deg,
    rgba(241, 145, 139, 1) 0%,
    rgba(238, 95, 83, 1) 100%
  );
  background: -webkit-linear-gradient(
    90deg,
    rgba(241, 145, 139, 1) 0%,
    rgba(238, 95, 83, 1) 100%
  );
  background: linear-gradient(
    90deg,
    rgba(241, 145, 139, 1) 0%,
    rgba(238, 95, 83, 1) 100%
  );
  filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#f1918b",endColorstr="#ee5f53",GradientType=1);
}

.input {
  font-size: 23px;
  padding: 8px;
  display: block;
  border: none;
  border-bottom: 1px solid #173353;
  border-radius: 4px;
  color: #173353 !important;
  box-shadow: 5px -3px 7px #173353;
  margin: 10px 0px;
  /* width: 50%; */
}

.error-input {
  color: #ffffff !important;
  background: repeating-linear-gradient(
    45deg,
    #37506e,
    #37506e 10px,
    #173353 10px,
    #173353 20px
  );
}

.error-message {
  font-size: 14px;
}

@media (max-width: 470px) {
  .input {
    font-size: 18px;
    padding: 6px;
    width: 90%;
  }
}

.input ::-webkit-input-placeholder {
  color: #74869b;
  font-style: italic;
  padding-left: 3px;
}

.input:-ms-input-placeholder {
  color: #74869b;
  font-style: italic;
  padding-left: 3px;
}

.input::placeholder {
  color: #74869b;
  font-style: italic;
  padding-left: 3px;
}

.lable {
  font-size: 23px;
  margin-right: 30px;
  color: #173353 !important;
}

@media (max-width: 470px) {
  .lable {
    font-size: 18px;
    margin-right: 20px;
  }
}

@media (max-width: 325px) {
  .lable {
    display: none;
  }
}
</style>
