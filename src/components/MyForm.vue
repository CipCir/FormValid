/* eslint-disable no-debugger */
<template>
  <form class="Myform" autocomplete="off">
    <div
      class="fieldCont"
      v-for="(keyValue, objKey) in FieldsObj"
      :key="objKey"
    >
      <label :for="objKey">{{ objKey }}</label>
      <textarea
        v-if="objKey === 'Adress'"
        :id="objKey"
        class="formInput"
        v-model="keyValue.val"
        @focus="HandleFocus(keyValue)"
      />
      <input
        v-else
        :id="objKey"
        class="formInput"
        type="text"
        v-model="keyValue.val"
        @focus="HandleFocus(keyValue)"
      />
      <span class="ErrMsg" v-if="keyValue.hasErr">{{ validMsg }}</span>
    </div>
    <div class="btn" @click="ValidationHandler">Validate</div>
    <div class="fieldCont formStatus" v-if="showStatus && formStatus">
      Form is valid
    </div>
  </form>
</template>

<script>
export default {
  name: "MyForm",
  props: {
    validMsg: String,
  },
  data() {
    return {
      FieldsObj: {
        Name: { val: "", visited: false, hasErr: false },
        Email: { val: "", visited: false, hasErr: false },
        Adress: { val: "", visited: false, hasErr: false },
      },
      showStatus: false,
    };
  },
  computed: {
    formStatus() {
      let myObj = this.FieldsObj;
      let letFormValid = true;
      Object.keys(myObj).forEach((key) => {
        if (myObj[key].hasErr) {
          letFormValid = false;
        }
      });
      return letFormValid;
    },
  },
  methods: {
    ValidationHandler() {
      this.showStatus = true;
      let myObj = this.FieldsObj;
      Object.keys(myObj).forEach((key) => {
        if (myObj[key].visited && myObj[key].val === "") {
          myObj[key].hasErr = true;
        } else {
          myObj[key].hasErr = false;
        }
      });
    },
    HandleFocus(keyValue) {
      keyValue.visited = true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
form {
  border: solid 1px grey;
}
.fieldCont {
  display: flex;
  margin: 10px;
}
.formInput {
  margin-left: 10px;
  background-color: transparent;
  border: none;
  border-bottom: 1px solid #9e9e9e;
  border-radius: 0;
  outline: none;
  font-size: 16px;
  padding: 0;
  box-shadow: none;
  box-sizing: content-box;
  transition: border 0.3s, -webkit-box-shadow 0.3s;
  transition: box-shadow 0.3s, border 0.3s;
  transition: box-shadow 0.3s, border 0.3s, -webkit-box-shadow 0.3s;
}
.formInput:focus {
  border-bottom: 1px solid #26a69a;
  box-shadow: 0 1px 0 0 #26a69a;
}
.btn {
  padding: 10px;
  width: 300px;
  margin: 10px auto;
  cursor: pointer;
  user-select: none;
  vertical-align: middle;
  z-index: 1;
  transition: 0.3s ease-out;
  text-decoration: none;
  color: #fff;
  background-color: #26a69a;
  text-align: center;
  letter-spacing: 0.5px;
  font-size: 14px;
  outline: 0;
  box-shadow: 0 2px 2px 0 rgb(0 0 0 / 14%), 0 3px 1px -2px rgb(0 0 0 / 12%),
    0 1px 5px 0 rgb(0 0 0 / 20%);
}
.ErrMsg {
  color: white;
  font-size: small;
  background: red;
  padding: 2px 5px;
  border-radius: 5px;
  margin-left: 5px;
  height: 18px;
}
.formStatus {
  color: green;
}
</style>
