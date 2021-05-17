<template>
  <div class="d-flex d-flex-column align-center justify-center mb-20">
    <strong
      ><div class="title mt-20 mb-20">
        GET IN TOUCH
      </div></strong
    >
    <arrow-down class="mb-20"></arrow-down>
    <form
      @submit.prevent="submit"
      ref="form"
      class="d-flex d-flex-column justify-center align-center"
    >
      <div class="d-flex align-center" style="width: 100%;">
        <label for="name-contact" class="lable">Name:</label>
        <div>
          <input
            :class="`${isNameValid.error ? 'error-input' : ''}`"
            @focus="isNameValid.focus = true"
            @blur="isNameValid.blur = true"
            style="width: 100%;"
            ref="name-contact"
            id="name-contact"
            type="text"
            placeholder="John Smith"
            class="input"
            name="name-contact"
            v-model.trim="name"
          />
          <div class="error-message">{{ isNameValid.message }}</div>
        </div>
      </div>
      <div class="d-flex align-center" style="width: 100%;">
        <label for="email-contact" class="lable">E-mail:</label>
        <div>
          <input
            :class="`${isEmailValid.error ? 'error-input' : ''}`"
            @focus="isEmailValid.focus = true"
            @blur="isEmailValid.blur = true"
            style="width: 100%;"
            ref="email-contact"
            id="email-contact"
            type="text"
            placeholder="j.smith@gmail.com"
            class="input"
            name="email-contact"
            v-model.trim="email"
          />
          <div class="error-message">{{ isEmailValid.message }}</div>
        </div>
      </div>
      <div class="d-flex align-center" style="width: 100%;">
        <label for="mobile-contact" class="lable">Mobile:</label>
        <div>
          <input
            :class="`${isMobileValid.error ? 'error-input' : ''}`"
            @focus="isMobileValid.focus = true"
            @blur="isMobileValid.blur = true"
            style="width: 100%;"
            ref="mobile-contact"
            id="mobile-contact"
            type="text"
            placeholder="04 8888 8888"
            class="input"
            name="mobile-contact"
            v-model.trim="mobile"
          />
          <div class="error-message">{{ isMobileValid.message }}</div>
        </div>
      </div>
      <div class="d-flex align-center" style="width: 100%;">
        <label for="subject-contact" class="lable">Subject:</label>
        <div>
          <input
            :class="`${isSubjectValid.error ? 'error-input' : ''}`"
            @focus="isSubjectValid.focus = true"
            @blur="isSubjectValid.blur = true"
            style="width: 100%;"
            ref="subject-contact"
            id="subject-contact"
            type="text"
            placeholder="Super Star Blog!"
            class="input"
            name="subject-contact"
            v-model.trim="subject"
          />
          <div class="error-message">{{ isSubjectValid.message }}</div>
        </div>
      </div>
      <div class="d-flex align-start" style="width: 100%;">
        <label for="message-contact" class="lable mt-20">Message:</label>
        <div>
          <textarea
            :class="`${isMessageValid.error ? 'error-input' : ''}`"
            @focus="isMessageValid.focus = true"
            @blur="isMessageValid.blur = true"
            style="resize: none; width: 100%;"
            ref="message-contact"
            id="message-contact"
            rows="8"
            cols="19"
            placeholder="Say Hi!"
            class="input"
            name="message-contact"
            v-model.trim="message"
          >
          </textarea>
          <div class="error-message">{{ isMessageValid.message }}</div>
        </div>
      </div>
      <btn
        ref="submitButton"
        classes="mt-10 mb-10"
        subscribe
        subscribeBtnName="Send a message"
      />
    </form>
    <div class="mt-10">{{ sentMessage }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      email: "",
      mobile: "",
      subject: "",
      message: "",
      sentMessage: "",
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
      isMobileValid: {
        message: "",
        error: false,
        blur: false,
      },
      isSubjectValid: {
        message: "",
        error: false,
        blur: false,
      },
      isMessageValid: {
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
      this.applyMobileValidation();
      this.applySubjectValidation();
      this.applyMessageValidation();

      if (
        this.isNameValid.error ||
        this.isEmailValid.error ||
        this.isMobileValid.error ||
        this.isSubjectValid.error ||
        this.isMessageValid.error
      ) {
        return;
      }

      // send the new subscriber's data to the firestore
      await this.sendToFirebase();
      // send an email to me
      this.sendingEmailToMe();
      this.sentMessage =
        "Thank you for contacting me! I`ll get in touch with you as soon as possible!";
      setTimeout(() => {
        this.sentMessage = "";
      }, 5000);
      //restart the form
      this.resetForm();
    },
    applyNameValidation(reset = false) {
      if (this.name.trim() === "" && !reset) {
        this.isNameValid.message = "The Name field is required!";
        this.isNameValid.error = true;
      } else {
        this.isNameValid.message = "";
        this.isNameValid.blur = false;
        this.isNameValid.error = false;
      }
    },
    applyEmailValidation(reset = false) {
      if (this.email.trim() === "" && !reset) {
        this.isEmailValid.message = "The Email field is required!";
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
    applyMobileValidation(reset = false) {
      if (this.mobile.trim() === "" && !reset) {
        this.isMobileValid.message = "The Mobile field is required!";
        this.isMobileValid.error = true;
      } else if (!/^[0-9]+$/.test(this.mobile.trim()) && !reset) {
        this.isMobileValid.message = "Only numeric values are allowed!";
        this.isMobileValid.error = true;
      } else if (!/\d{8,12}/.test(this.mobile.trim()) && !reset) {
        this.isMobileValid.message = "Between 8 to 12 digits allowed!";
        this.isMobileValid.error = true;
      } else {
        this.isMobileValid.message = "";
        this.isMobileValid.blur = false;
        this.isMobileValid.error = false;
      }
    },
    applySubjectValidation(reset = false) {
      if (this.subject.trim() === "" && !reset) {
        this.isSubjectValid.message = "The Subject field is required!";
        this.isSubjectValid.error = true;
      } else {
        this.isSubjectValid.message = "";
        this.isSubjectValid.blur = false;
        this.isSubjectValid.error = false;
      }
    },
    applyMessageValidation(reset = false) {
      if (this.message.trim() === "" && !reset) {
        this.isMessageValid.message = "The Message field is required!";
        this.isMessageValid.error = true;
      } else if (this.message.length > 500 && !reset) {
        this.isMessageValid.message = "No more than 500 characters allowed!";
        this.isMessageValid.error = true;
      } else {
        this.isMessageValid.message = "";
        this.isMessageValid.blur = false;
        this.isMessageValid.error = false;
      }
    },
    resetForm() {
      this.name = "";
      this.email = "";
      this.mobile = "";
      this.subject = "";
      this.message = "";
      this.$nextTick(() => {
        this.$refs["name-contact"].blur();
        this.$refs["email-contact"].blur();
        this.$refs["mobile-contact"].blur();
        this.$refs["subject-contact"].blur();
        this.$refs["message-contact"].blur();
        this.applyNameValidation(true);
        this.applyEmailValidation(true);
        this.applyMobileValidation(true);
        this.applySubjectValidation(true);
        this.applyMessageValidation(true);
      });
    },
    sendToFirebase() {
      return new Promise((resolve, reject) => {
        firestore
          .collection("contact-form")
          .add({
            Name: this.name,
            Email: this.email.toLowerCase(),
            Mobile: this.mobile,
            Subject: this.subject,
            Message: this.message,
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
        const sendContactFormEmail = functions.httpsCallable(
          "sendContactFormEmail"
        );
        sendContactFormEmail({
          Name: this.name,
          Email: this.email.toLowerCase(),
          Mobile: this.mobile,
          Subject: this.subject,
          Message: this.message,
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
    mobile() {
      this.applyMobileValidation();
    },
    subject() {
      this.applySubjectValidation();
    },
    message() {
      this.applyMessageValidation();
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
    "isMobileValid.blur": {
      handler() {
        this.applyMobileValidation();
      },
    },
    "isSubjectValid.blur": {
      handler() {
        this.applySubjectValidation();
      },
    },
    "isMessageValid.blur": {
      handler() {
        this.applyMessageValidation();
      },
    },
  },
};
</script>

<style scoped>
.title {
  font-size: 36px;
  line-height: 35px;
  text-align: center;
}

@media (max-width: 470px) {
  .title {
    font-size: 28px;
    line-height: 30px;
  }
}

.input {
  font-size: 23px;
  padding: 8px;
  display: block;
  border: none;
  border-bottom: 1px solid #9b9b9b;
  border-radius: 4px;
  color: #173353 !important;
  box-shadow: 5px -3px 7px #9b9b9b;
  margin: 10px 0px;
}

@media (max-width: 470px) {
  .input {
    font-size: 18px;
    padding: 6px;
    width: 65%;
  }
}

@media (max-width: 325px) {
  .input {
    width: 90%;
  }
}

.input ::-webkit-input-placeholder {
  color: #9b9b9b;
  font-style: italic;
  padding-left: 3px;
}

.input:-ms-input-placeholder {
  color: #9b9b9b;
  font-style: italic;
  padding-left: 3px;
}

.input::placeholder {
  color: #9b9b9b;
  font-style: italic;
  padding-left: 3px;
}

.lable {
  font-size: 23px;
  margin-right: 30px;
  width: 25%;
  color: #6d6e71 !important;
}

@media (max-width: 470px) {
  .lable {
    font-size: 18px;
    margin-right: 20px;
    width: 30%;
  }
}

@media (max-width: 325px) {
  .lable {
    display: none;
  }
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
</style>
