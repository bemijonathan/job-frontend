<template>
  <div id="app">
    <form>
      <select @click="showCheckboxes">
        <option>Select an option</option>
      </select>
      <div id="checkboxes" :class="{ showclick: clickSelect }">
        <label for="one">
          <input
            type="checkbox"
            id="one"
            value="australia"
            v-model="filter"
          />Australia</label
        >
        <label for="two">
          <input
            type="checkbox"
            id="two"
            value="canada"
            v-model="filter"
          />Canada</label
        >
        <label for="three">
          <input
            type="checkbox"
            id="three"
            value="usa"
            v-model="filter"
          />Usa</label
        >
      </div>
    </form>

    <table id="customers">
      <tr>
        <th>Company</th>
        <th>Contact</th>
        <th>Country</th>
      </tr>
      <tr v-for="(data, i) in secondData" :key="i">
        <td>{{ data.Company }}</td>
        <td>{{ data.Contact }}</td>
        <td>{{ data.Country }}</td>
      </tr>
    </table>

    <div>
      <form @submit.prevent="submit">
        <label for="Company">Company</label>
        <input
          type="text"
          id="Company"
          v-model="Company"
          name="firstname"
          placeholder="Your Company.."
        />

        <label for="Contact">Contact</label>
        <input
          type="text"
          id="Contact"
          v-model="Contact"
          name="lastname"
          placeholder="Your Contact.."
        />

        <label for="Country">Country</label>
        <select id="Country" name="Country" v-model="Country">
          <option value="australia">Australia</option>
          <option value="canada">Canada</option>
          <option value="usa">USA</option>
        </select>

        <input type="submit" value="Submit" />
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { filter } from "minimatch";
export default {
  data() {
    return {
      datas: [],
      Country: "",
      Company: "",
      Contact: "",
      secondData: [],
      clickSelect: false,
      filter: []
    };
  },
  created() {
    this.getData();
  },
  methods: {
    submit() {
      axios
        .post("https://bemijoe.herokuapp.com/", {
          Contact: this.Contact,
          Country: this.Country,
          Company: this.Company
        })
        .then(user => {
          console.log(user);

          this.getData();
        });
    },
    showCheckboxes() {
      this.clickSelect = !this.clickSelect;
    },
    getData() {
      axios.get("https://bemijoe.herokuapp.com/").then(data => {
        this.datas = data.data;
        this.secondData = data.data;
        console.log(data);
      });
    }
  },
  watch: {
    filter() {
      var aaa = this.datas.filter(a => {
        return this.filter.includes(a.Country);
      });

      if (aaa.length === 0) {
        console.log(aaa);
        this.secondData = this.datas;
      } else {
        this.secondData = aaa;
      }
    }
  }
};
</script>

<style>
.showclick {
  display: grid !important;
}
/* .multiselect {
  width: 200px;
}

.selectBox {
  position: relative;
}

.selectBox select {
  width: 100%;
  font-weight: bold;
}

.overSelect {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
}*/

#checkboxes {
  display: none;
  border: 1px #dadada solid;
}
/*
checkboxes label {
  display: block;
}

#checkboxes label:hover {
  background-color: #1e90ff;
} */

input[type="text"],
select {
  width: 100%;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}

input[type="submit"] {
  width: 100%;
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

input[type="submit"]:hover {
  background-color: #45a049;
}

div {
  border-radius: 5px;
  background-color: #f2f2f2;
  padding: 20px;
}
#customers {
  font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#customers td,
#customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

#customers tr:nth-child(even) {
  background-color: #f2f2f2;
}

#customers tr:hover {
  background-color: #ddd;
}

#customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #4caf50;
  color: white;
}
</style>
