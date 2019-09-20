<template>

  <div class="homePage">

    <!-- Search Box -->
    <div class="exploreBox">

      <!-- Header -->
      <h1 class="boxHeader">Find places to stay on Airbnb</h1>

      <div class="headerDescription">Discover entire homes and private rooms perfect for any trip.</div>

      <!-- Form -->
      <v-form class="detailsForm" ref="form">
        <!-- Checkin Checkout -->
        <v-row>
          <!-- Checkin -->
          <v-col>
            <v-menu v-model="checkinMenu" :close-on-content-click="false" transition="scale-transition" offset-y full-width max-width="290px" min-width="290px">
              <template v-slot:activator="{ on }">
                <v-text-field v-model="checkinDate" label="CHECK-IN" prepend-inner-icon="event" outlined readonly v-on="on"></v-text-field>
              </template>
              <v-date-picker v-model="checkinDate" no-title @input="checkinMenu = false"></v-date-picker>
            </v-menu>
            </v-layout>
          </v-col>
          <!-- Checkout -->
          <v-col>
            <v-menu v-model="checkoutMenu" :close-on-content-click="false" transition="scale-transition" offset-y full-width max-width="290px" min-width="290px">
              <template v-slot:activator="{ on }">
                <v-text-field v-model="checkoutDate" label="CHECK-OUT" prepend-inner-icon="event" outlined readonly v-on="on"></v-text-field>
              </template>
              <v-date-picker v-model="checkoutDate" no-title @input="checkoutMenu = false"></v-date-picker>
            </v-menu>
            </v-layout>
          </v-col>
        </v-row>

        <v-combobox v-model="homeList" chips clearable label="Add listing ids here" multiple outlined>
          <template v-slot:selection="{ attrs, item, select, selected }">
            <v-chip v-bind="attrs" :input-value="selected" close @click="select" @click:close="removeListingItem(item)">
              <strong>{{ item }}</strong>
            </v-chip>
          </template>
        </v-combobox>

        <!-- Search Button -->
        <div class="searchBtnContainer">
          <v-btn @click="search" prepend-inner-icon="magnify" depressed x-large large :loading="fetchingData">
            <v-icon left>mdi-magnify</v-icon>
            Search
          </v-btn>
        </div>

      </v-form>

    </div>

    <!-- Result Table -->
    <div class="resultList">
      <table class="resultTable" v-if="!fetchingData && Object.keys(resultList).length > 0">
        <thead>
          <tr>
            <th>Listing Id</th>
            <th v-for="(date, index) in headers" :key="index">
              {{date}}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(data, listingId) in resultList">
            <td>
              <a :href="data.url">{{listingId}}</a>
            </td>
            <td v-for="(date, index1) in data.dayList" :key="index1" v-bind:class="{'success': date.available, 'failure': !date.available}">
              {{date.price.local_price_formatted}}
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <v-snackbar v-model="showSnackbar" color="#B71C1C">
      {{ errorMessage }}
      <v-btn color="pink" text @click="showSnackbar = false">
        Close
      </v-btn>
    </v-snackbar>

  </div>

</template>

<script>
import axios from "axios";
import moment from "moment";

export default {
  data: () => ({
    homeList: [
      "https://www.airbnb.co.in/rooms/16420029?toddlers=0&adults=1&check_in=2019-09-19&check_out=2019-09-27&source_impression_id=p3_1568899349_r6SymJA8duQO2mg5"
    ],

    adults: null,
    adultsList: [
      { value: 1, label: "1 adult" },
      { value: 2, label: "2 adults" },
      { value: 3, label: "3 adults" },
      { value: 4, label: "4 adults" },
      { value: 5, label: "5 adults" },
      { value: 6, label: "6 adults" },
      { value: 7, label: "7 adults" },
      { value: 8, label: "8 adults" },
      { value: 9, label: "9 adults" },
      { value: 10, label: "10 adults" },
      { value: 11, label: "11 adults" },
      { value: 12, label: "12 adults" },
      { value: 13, label: "13 adults" },
      { value: 14, label: "14 adults" },
      { value: 15, label: "15 adults" },
      { value: 16, label: "16 adults" }
    ],

    childs: null,
    childsList: [
      { value: 1, label: "1 child" },
      { value: 2, label: "2 childs" },
      { value: 3, label: "3 childs" },
      { value: 4, label: "4 childs" },
      { value: 5, label: "5 childs" }
    ],

    checkinDate: "2019-09-25",
    // checkinDate: null,
    checkinMenu: false,

    checkoutDate: "2019-09-29",
    // checkoutDate: null,
    checkoutMenu: false,

    fetchingData: false,
    showSnackbar: false,
    errorMessage: false,
    headers: [],
    resultList: {}
  }),
  methods: {
    async search() {
      try {
        // Validate listing
        if (this.homeList.length == 0) {
          this.showSnackbar = true;
          this.errorMessage = "Provide atleast one listing to check.";
          return;
        } else if (this.homeList.length > 10) {
          this.showSnackbar = true;
          this.errorMessage = "Max 10 listings are allowed to check.";
          return;
        }

        // Validate start end date
        let startDate = moment(this.checkinDate);
        let endDate = moment(this.checkoutDate);
        if (!startDate.isValid() || !startDate.isValid()) {
          this.showSnackbar = true;
          this.errorMessage = "Invalid check-in or check-out date.";
          return;
        } else if (startDate > endDate) {
          this.showSnackbar = true;
          this.errorMessage = "Start date can not be greater than end date.";
          return;
        } else if (startDate < moment()) {
          this.showSnackbar = true;
          this.errorMessage =
            "Please provide future date, current or past dates are not allowed.";
          return;
        }

        let lst = [],
          listUrlMap = {};
        for (let i = 0; i < this.homeList.length; i++) {
          let ddd = new URL(this.homeList[i]);
          let id = ddd.pathname.replace("/rooms/", "");
          listUrlMap[id] = this.homeList[i];
          lst.push(id);
        }

        this.fetchingData = true;
        let params = {
          checkinMonth: startDate.get("month") + 1,
          // "checkinMonth": 9,
          checkinYear: startDate.get("year"),
          listings: lst
        };

        let url =
          "https://vwx50cjyk0.execute-api.us-east-1.amazonaws.com/dev/search";
        let response = (await axios(url, {
          method: "POST",
          data: params
          // withCredentials: true
        })).data;

        let listWiseAvailablity = {};
        let listingList = Object.keys(response);
        for (var i = 0; i < listingList.length; i++) {
          let listingId = listingList[i];
          let availablityInfo = response[listingId].calendar_months;
          listWiseAvailablity[listingId] = {
            url: listUrlMap[listingId],
            dayList: []
          };

          for (var j = 0; j < availablityInfo.length; j++) {
            let monthObject = availablityInfo[j];

            // If year and months match, compare days.
            console.log(
              "comparing days for",
              monthObject.year,
              monthObject.month
            );
            let dayList = monthObject.days;
            for (var k = 0; k < dayList.length; k++) {
              let dayObject = dayList[k];
              let currentDate = moment(dayObject.date);
              if (
                currentDate.isBetween(startDate, endDate) ||
                currentDate.isSame(startDate) ||
                currentDate.isSame(endDate)
              ) {
                console.log("consider date", currentDate);
                listWiseAvailablity[listingId].dayList.push(dayObject);
              }
            }
          }
        }

        // Build headers
        let headers = [];
        let diff = endDate.diff(startDate, "days");
        let dt = startDate.clone();
        for (var i = 0; i <= diff; i++) {
          headers.push(dt.format("YYYY-MM-DD"));
          dt = dt.add(1, "days");
        }
        this.headers = headers;

        this.resultList = listWiseAvailablity;
        this.fetchingData = false;
        console.log("showing results");
      } catch (err) {
        console.log("failed", err, err.message, err.response);
        this.fetchingData = false;
      }
    },

    removeListingItem(item) {
      this.homeList.splice(this.homeList.indexOf(item), 1);
      this.homeList = [...this.homeList];
    }
  }
};
</script>

<style lang="scss">
::-webkit-scrollbar {
  display: none;
}

body {
  font-family: roboto;

  .homePage {
    background-image: url("https://images.unsplash.com/photo-1551516595-097a8d938ecd?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1502&q=80");
    background-repeat: no-repeat;
    background-size: cover;
    background-position: center center;
    min-height: 100vh;
    padding-top: 10vh;

    .exploreBox {
      width: 80%;
      padding: 25px;
      opacity: 0.94;
      margin: auto;

      background: white;
      border-radius: 3px;

      .boxHeader {
        color: #555;
        font-weight: 400;
      }

      .headerDescription {
        font-weight: 300;
        margin: 5px 0px 10px 0px;
      }

      .detailsForm {
        margin-top: 25px;

        .v-text-field__details {
          display: none;
        }

        .searchBtnContainer {
          text-align: right;

          button {
            background: #009688 !important;
            color: white !important;
          }
        }
      }
    }

    .resultList {
      max-width: 80%;
      margin: auto;
      margin-top: 5vh;
      opacity: 0.94;
      overflow-x: scroll;

      background: white;
      border-radius: 3px;

      .resultTable {
        border-collapse: collapse;

        th,
        td {
          border: 1px solid #ddd;
          border-collapse: none;
          padding: 5px 3px 5px 5px;
          line-height: 25px;
          text-align: left;
          min-width: 200px;
        }

        .failure {
          color: white;
          background: red;
        }

        .success {
          background: green;
          // color: white;
        }
      }
    }
  }
}
</style>