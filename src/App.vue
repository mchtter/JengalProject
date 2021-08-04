<template>
  <div id="app">
    <div class="row justify-content-center p-5">
      <div class="search-box justify-content-center">
        <button class="btn-search"><i class="fas fa-search"></i></button>
        <input
          type="text"
          class="input-search"
          placeholder="Type to Search..."
          v-model="nameToSearchString"
        />
      </div>
    </div>

    <main class="page-content volunteership">
      <div
        v-for="(data, index) in searchData"
        v-bind:key="index._id"
        class="card volunteerCard"
      >
        <img
          :src="data.picture"
          height="200"
          width="65"
          class="card-img"
          style="border-radius: 10px"
        />
        <div class="content">
          <h2 class="volunteerTitle">{{ data.name }}</h2>
          <br />
          <p class="copy">
            {{ data.balance }}
          </p>
          <p class="copy">
            {{ data.company }}
            {{ data.email }}
          </p>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      nameToSearchString: "",
      allData: null,
    };
  },
  async created() {
    await this.getData();
  },
  methods: {
    getData() {
      axios.get("assets/db/data.json").then((response) => {
        this.allData = response.data;
      });
    },
  },
  computed: {
    searchData: function () {
      let data = this.allData;
      let nameToSearchString = this.nameToSearchString;

      if (!nameToSearchString) {
        return data;
      }

      nameToSearchString = nameToSearchString.trim().toLowerCase();

      data = data.filter(function (item) {
        var name = item.name.toLowerCase().indexOf(nameToSearchString);
        var company = item.company.toLowerCase().indexOf(nameToSearchString);
        var balance = item.balance.toLowerCase().indexOf(nameToSearchString);
        var email = item.email.toLowerCase().indexOf(nameToSearchString);

        if (name !== -1 || company !== -1 || balance !== -1 || email !== -1) {
          return item;
        }
      });

      return data;
    },
  },
  components: {},
};
</script>