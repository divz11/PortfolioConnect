<!-- <button type="menu">+</button> -->
<template>
  <div class="create">
    <form @submit.prevent="Create()" class="create-form">
      <table class="create-form-text">
        <tr>
          <td>
            <select
              name="honorific-prefix"
              id="honorific-prefix"
              v-model="Salutation"
            >
              <option selected disabled value="select-salutation">
                Select Salutation
              </option>
              <option
                v-for="(i, index) in HonorificPrefixList"
                :key="index"
                :value="i.value"
              >
                {{ i.text }}
              </option>
            </select>
          </td>
          <td>
            <input
              name="full-name"
              type="text"
              v-model="FullName"
              placeholder="Full Name"
              disabled
            />
          </td>
          <td>
            <input name="age" type="text" v-model="Age" placeholder="Age" />
          </td>
          <td>
            <input
              name="tel-personal"
              type="tel"
              v-model="PhoneNumber"
              placeholder="Phone Number"
              disabled
            />
          </td>
          <td>
            <input
              name="email"
              type="text"
              v-model="Email"
              placeholder="Email"
              disabled
            />
          </td>
          <td>
            <select
              name="qualification"
              id="qualification"
              v-model="Qualification"
            >
              <option selected disabled value="select-qualification">
                Select Qualification
              </option>
              <option value="Engineering">Engineering</option>
              <option value="Bachelor In Computer Application">
                Bachelor In Computer Application
              </option>
              <option value="Bachelor In Commerce">Bachelor In Commerce</option>
              <option value="Bachelor In Business Administrative">
                Bachelor In Business Administrative
              </option>
            </select>
          </td>
        </tr>
        <tr>
          <td>
            <select name="university" id="university" v-model="University">
              <option selected disabled value="select-university">
                Select University
              </option>
              <option
                v-for="(i, index) in UniversityList"
                :key="index"
                :value="i.value"
              >
                {{ i.text }}
              </option>
            </select>
          </td>
          <td>
            <input
              type="text"
              placeholder="Professional Title"
              v-model="ProfessionalTitle"
            />
          </td>
          <td>
            <textarea
              cols="30"
              rows="5"
              placeholder="Short and engaging pitch about yourself..."
              v-model="Pitch"
            ></textarea>
          </td>
        </tr>
        <tr>
          <td>
            <skill-set
              class="skill-set"
              @SkillsUpdate="HandleUpdate"
            ></skill-set>
          </td>
        </tr>
        <tr v-for="(Count, index) in Counters.Organization" :key="index">
          <td>
            <input type="text" placeholder="Organization" />
          </td>
          <td>
            <input
              type="text"
              maxlength="10"
              minlength="10"
              pattern="[0-9/]{10}"
              placeholder="Start Date (DD/MM/YYYY)"
            />
          </td>
          <td>
            <input
              type="text"
              maxlength="10"
              minlength="10"
              placeholder="End Date (DD/MM/YYYY)"
            />
          </td>
          <td>
            <textarea
              cols="30"
              rows="5"
              placeholder="Your Experience and Achivements"
            ></textarea>
          </td>
        </tr>
      </table>
      <button @click="Counters.Organization++" class="add-button">+</button>
      <span>
        <input type="text" placeholder="Github Profile ID" />
        <input type="text" placeholder="LinkedIn Profile ID" />
      </span>
      <span class="certifcates">
        <div v-for="i in Counters.Certificate" :key="i">
          <label class="button" for="Certificate">
            Upload Your Certificates
          </label>
          <input
            type="file"
            id="Certificate"
            class="hidden"
            name="Upload"
            accept="application/pdf"
          />
        </div>
        <button @click="Counters.Certificate++" class="add-button">
          &#9769;
        </button>
      </span>
      <button type="submit" class="submit-button">SUBMIT &#8595;</button>
    </form>
    <div v-if="Error" class="error">{{ Error }}</div>
  </div>
</template>

<script>
import SkillSet from "../components/create/SkillSet";
export default {
  data() {
    return {
      UniversityList: [
        { text: "BIT Mesra", value: "BIT Mesra" },
        { text: "PES University", value: "PES University" },
      ],
      HonorificPrefixList: [
        { value: "Mr.", text: "Mr." },
        { value: "Ms.", text: "Ms." },
        { value: "Miss", text: "Miss" },
        { value: "Mrs.", text: "Mrs." },
        { value: "Dr.", text: "Dr." },
      ],
      Salutation: "select-salutation",
      FullName: "",
      Age: "",
      PhoneNumber: "",
      Email: "",
      Qualification: "select-qualification",
      University: "select-university",
      SkillSet: [],
      ProfessionalTitle: "",
      Pitch: "",
      Error: false,
      Counters: { Organization: 1, Certificate: 1 },
    };
  },
  components: {
    SkillSet,
  },
  methods: {
    HandleUpdate(event) {
      const { value } = event.target;
      this.SkillSet.push(value);
    },
    Create() {
      this.axios
        .post(
          "/portfolios",
          {
            salutation: this.Salutation,
            fullName: this.FullName,
            age: this.Age,
            phoneNumber: this.PhoneNumber,
            email: this.Email,
            qualification: this.Qualification,
            university: this.University,
            skillSet: this.SkillSet,
          },
          {
            headers: {
              "Content-Type": "application/json;charset=UTF-8",
              "Access-Control-Allow-Origin": "*",
              Authorization: localStorage["jwt"],
            },
          }
        )
        .then(
          (res) => {
            // successful login
            // a non-200 response code
            this.Error = res.data.message;
          },
          (err) => {
            console.log(err.response.data.message);
            this.Error = err.response.data.message.replace(/"/g, "");
          }
        );
    },
    LoginCheck() {
      if (localStorage["jwt"] == undefined) this.$router.push("/");
    },
  },
  mounted() {
    this.LoginCheck();
    this.FullName = this.$store.state.User["FullName"];
    this.PhoneNumber = this.$store.state.User["PhoneNumber"];
    this.Email = this.$store.state.User["Email"];
  },
};
</script>

<style lang="scss" scoped>
@media screen and (min-width: 1080px) {
  .create {
    height: 100%;
    width: 100%;
    padding: 150px 0px 35px 0px;
    background: linear-gradient(
      to right bottom,
      #3103ae,
      #2b04a0,
      #240492,
      #1f0484,
      #190477,
      #15056f,
      #110566,
      #0e055e,
      #0b0659,
      #090654,
      #070650,
      #06054b
    );
    &-form {
      margin: auto;
      max-width: 80vw;
      border: 3px solid #e3507d;
      &-text {
        display: flex;
        flex-flow: row wrap;
        justify-content: space-around;
        align-items: stretch;
        padding: 50px;
        tr {
          margin: 60px 0px;
          display: flex;
          flex-flow: column wrap;
          border: 3px solid #e3507d;
          padding: 50px;
          min-width: 500px;
          td {
            margin: 10px;
            .hidden {
              display: none;
            }
          }
        }
      }
      input,
      select,
      textarea {
        background-color: transparent;
        color: #e3507d;
        padding: 5px;
        font-weight: 600;
        font-size: 17px;
        border: none;
        border-bottom: 2px solid #e3507d;
        &:focus,
        &:hover {
          box-shadow: 5px 7px 5px 4px rgba(0, 0, 0, 0.5);
        }
      }
      input {
        width: 400px;
        height: 30px;
      }
      select {
        width: 409px;
        height: 45px;
        // appearance: none;
        option {
          background: rgba(227, 80, 125, 0.6) !important;
        }
      }
      textarea {
        width: 370px;
        margin: 50px;
        color: #e3507d;
        padding: 20px;
        display: block;
        &::placeholder {
          padding: 10px;
        }
      }
      button {
        background-color: #140156;
        border: 3px solid #e3507d;
        font-weight: 600;
        padding: 7px;
        color: #e3507d;
        cursor: pointer;
        &:hover {
          background-color: #e3507d;
          color: whitesmoke;
        }
      }
      .add-button {
        width: 50px;
        border-radius: 50px;
      }
      span {
        display: block;
        margin: 100px;
      }
      .submit-button {
        width: 260px;
        margin: 10px;
        font-size: 17px;
        background-color: #140156;
        border: 2px solid #e3507d;
      }
    }
  }
}
</style>
