<template>
  <div id="app">
    <table id="customers">
      <tr>
        <th>Company</th>
        <th>Contact</th>
        <th>Country</th>
      </tr>
      <tr v-for="(data, i) in datas" :key="i">
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
export default {
  data() {
    return {
      datas: [],
      Country: "",
      Company: "",
      Contact: ""
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
    getData() {
      axios.get("https://bemijoe.herokuapp.com/").then(data => {
        this.datas = data.data;
        console.log(data);
      });
    }
  }
};
</script>

<style>
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
