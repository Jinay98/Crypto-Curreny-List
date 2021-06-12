<template>
  <div class="card text-center m-3">
    <h3 class="card-header">Crypto Curreny List</h3>
    <div style="background-color: rgba(0, 0, 0, 0.2)">
      <div class="card" style="width: 60%; margin: 0 auto">
        <table class="card-table table">
          <thead>
            <tr>
              <th scope="col">Name</th>
              <th scope="col">Symbol</th>
              <th scope="col">Price</th>
              <th scope="col">Price Change</th>
              <th scope="col">Icon</th>
              <th>
                <div>
                  <b-dropdown
                    id="dropdown-1"
                    text="No of Records"
                    class="m-md-2"
                  >
                    <b-dropdown-item disabled value="0"
                      >Select Page Size</b-dropdown-item
                    >
                    <b-dropdown-item
                      v-for="option in this.dropdownOptions"
                      :key="option.value"
                      :value="option.value"
                      @click="setNoOfEntries(option.value)"
                    >
                      {{ option.text }}
                    </b-dropdown-item>
                  </b-dropdown>
                </div>
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in pageOfItems" :key="item.id">
              <td>{{ item.name }}</td>

              <td>{{ item.symbol }}</td>
              <td>{{ Math.round(parseFloat(item.price) * 1000) / 1000 }}</td>
              <td>{{ item.change }}%</td>
              <td>
                <img :src="item.iconUrl" style="width: 25%; height: 50px" />
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="card-footer pb-0 pt-3">
      <jw-pagination
        :pageSize="this.noOfEntries"
        :items="exampleItems"
        @changePage="onChangePage"
      ></jw-pagination>
    </div>
  </div>
</template>

<script>
const exampleItems = [];
export default {
  data() {
    return {
      dropdownOptions: [
        {
          value: 10,
          text: "10",
        },
        {
          value: 25,
          text: "25",
        },
        {
          value: 50,
          text: "50",
        },
        {
          value: 100,
          text: "100",
        },
      ],
      noOfEntries: 10,
      exampleItems,
      pageOfItems: [],
    };
  },
  mounted() {
    this.getEndpointData()
  },
  methods: {
    onChangePage(pageOfItems) {
      this.pageOfItems = pageOfItems;
    },
    setNoOfEntries(pageSize) {
      console.log(pageSize);
      console.log(this.noOfEntries);
      this.noOfEntries = pageSize;
      console.log(this.noOfEntries);
      this.getEndpointData()
    },
    getEndpointData() {
      var url = "https://api.coinranking.com/v1/public/coins/?limit=100";
      fetch(url)
        .then((res) => res.json())
        .then((data) => (this.exampleItems = data.data.coins))
        .catch((err) => console.log(err.message));
    },
  },
};
</script>
