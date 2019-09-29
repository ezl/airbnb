<template>

  <div class="homePage">

    <!-- Search Box -->
    <div class="exploreBox">

      <!-- Header -->
      <h1 class="boxHeader">Combine Multiple Airbnb Stays</h1>

      <div class="headerDescription">If you can't find one place that's available for your whole trip, add several to see overlapping availability.</div>

      <!-- Form -->
      <v-form class="detailsForm" ref="form">
        <!-- Checkin Checkout -->
        <v-row>
          <!-- Checkin -->
          <v-col>
            <v-menu v-model="checkinMenu" :close-on-content-click="false" transition="scale-transition" offset-y max-width="290px" min-width="290px">
              <template v-slot:activator="{ on }">
                <v-text-field v-model="checkinDate" label="CHECK-IN" prepend-inner-icon="event" outlined readonly v-on="on"></v-text-field>
              </template>
              <v-date-picker v-model="checkinDate" no-title @input="checkinMenu = false"></v-date-picker>
            </v-menu>
            </v-layout>
          </v-col>
          <!-- Checkout -->
          <v-col>
            <v-menu v-model="checkoutMenu" :close-on-content-click="false" transition="scale-transition" offset-y max-width="290px" min-width="290px">
              <template v-slot:activator="{ on }">
                <v-text-field v-model="checkoutDate" label="CHECK-OUT" prepend-inner-icon="event" outlined readonly v-on="on"></v-text-field>
              </template>
              <v-date-picker v-model="checkoutDate" no-title @input="checkoutMenu = false"></v-date-picker>
            </v-menu>
            </v-layout>
          </v-col>
        </v-row>

        <v-combobox v-model="homeList" v-on:change="onTextAreaChange" chips clearable label="Add Airbnb listing URLs here" multiple outlined>
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
      <table class="resultTable" v-if="!fetchingData && resultList != null && Object.keys(resultList).length > 0">
        <thead>
          <tr>
            <th width="300">Listing
              <v-icon v-if="orderAscending" @click="OnChangeOrder()">arrow_upward</v-icon>
              <v-icon v-if="!orderAscending" @click="OnChangeOrder()">arrow_downward</v-icon>
              <!-- <i class="material-icons">
                arrow_downward
              </i> -->
            </th>
            <th v-for="(headerObj, index) in headers" :key="index">
              {{headerObj.text}}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(rowObj, index) in resultList" :key="index">
            <td>
              <a target="_blank" :href="'https://airbnb.com/rooms/' + rowObj.url">{{rowObj.name}}</a>

              <v-icon @click="resultList.splice(index, 1)">delete</v-icon>
            </td>
            <td v-for="(date, index1) in rowObj.data" :key="index1" v-bind:class="{'success': date.available, 'failure': !date.available}">
              {{date.price}}
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
import _ from "lodash";

export default {
  data: () => ({
    homeList: [],
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

    // checkinDate: "2019-09-25",
    checkinDate: null,
    checkinMenu: false,

    // checkoutDate: "2019-09-29",
    checkoutDate: null,
    checkoutMenu: false,

    fetchingData: false,
    showSnackbar: false,
    errorMessage: false,
    headers: [],
    resultList: null
  }),
  methods: {
    isValidListingUrl(url) {
      console.log("checking:", url)
      try {
        let ddd = new URL(url);
        let isAirBnB = ddd.hostname.indexOf("airbnb") !== -1;
        let isRoom= ddd.pathname.indexOf("rooms") !== -1;
        if (isAirBnB === false || isRoom === false) {
          console.log('values', isAirBnB, isRoom);
          throw "Invalid AirBnB listing link."
        }
      } catch (err) {
        console.log("failed", err, err.message, err.response);

        this.showSnackbar = true;
        this.errorMessage = '"' + url + '"' + " doesn't seem to be a valid AirBnB listing link.";
        return false;
      }

      return true;
    },

    cleanListingUrl(url) {
      console.log("cleaning url", url);
      return url;
    },

    onTextAreaChange() {
      console.log("textarea changed");
      console.log(this.homeList);
      this.homeList.forEach(function(item) {
        if (this.isValidListingUrl(item)) {
          console.log("url is valid:", item);
        } else {
          console.log("invalid url:", item);
          this.removeListingItem(item)
        }
      }, this);
    },

    async search() {
      try {
        // Validate listing
        if (this.homeList.length == 0) {
          this.showSnackbar = true;
          this.errorMessage = "Provide at least one listing to check.";
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
          this.errorMessage = "Hmmm... Invalid check-in or check-out date.";
          return;
        } else if (startDate > endDate) {
          this.showSnackbar = true;
          this.errorMessage =
            "Uh oh! Your start date can not be after your end date.";
          return;
        } else if (startDate < moment()) {
          this.showSnackbar = true;
          this.errorMessage =
            "Please provide a date in the future, current or past dates are not allowed.";
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

        // Fetch data from api.
        let url =
          "https://vwx50cjyk0.execute-api.us-east-1.amazonaws.com/dev/search";
        let response = (await axios(url, {
          method: "POST",
          data: params
          // withCredentials: true
        })).data;

        // Build Headers
        let headers = [];
        let diff = endDate.diff(startDate, "days");
        let dt = startDate.clone();
        for (var i = 0; i <= diff; i++) {
          // headers.push(dt.format("MM-DD"));
          headers.push({
            text: dt.format("MM-DD"),
            align: "left",
            sortable: false,
            value: dt.format("MM-DD")
          });
          dt = dt.add(1, "days");
        }
        this.headers = headers;

        let listWiseAvailablity = [];
        let listingList = Object.keys(response);
        for (var i = 0; i < listingList.length; i++) {
          let listingId = listingList[i];
          let availablityInfo = response[listingId].availablity.calendar_months;
          let listingName = response[listingId].name;

          let rowObject = {};
          rowObject["name"] = listingName.split("-")[0];
          rowObject["url"] = listingId;
          rowObject["data"] = [];

          for (var j = 0; j < availablityInfo.length; j++) {
            let monthObject = availablityInfo[j];

            // If year and months match, compare days.
            /*
            console.log(
              "comparing days for",
              monthObject.year,
              monthObject.month
            );
            */

            let dayList = monthObject.days;
            for (var k = 0; k < dayList.length; k++) {
              let dayObject = dayList[k];
              let currentDate = moment(dayObject.date);
              if (
                currentDate.isBetween(startDate, endDate) ||
                currentDate.isSame(startDate) ||
                currentDate.isSame(endDate)
              ) {
                // console.log("consider date", currentDate);
                // listWiseAvailablity[listingId].dayList.push(dayObject);
                let dateStr = currentDate.format("MM-DD");
                rowObject["data"].push({
                  available: dayObject.available,
                  price: dayObject.price.local_price_formatted
                });
              }
            }
          }

          listWiseAvailablity.push(rowObject);
        }

        this.resultList = listWiseAvailablity;
        this.fetchingData = false;
        console.log("showing results");
      } catch (err) {
        console.log("failed", err, err.message, err.response);
        this.fetchingData = false;
      }
    },

    OnChangeOrder() {
      debugger;
      this.orderAscending = !this.orderAscending;
      this.resultList = _.orderBy(this.resultList, ["name"], ["asc"]);
    },

    removeListingItem(item) {
      this.homeList.splice(this.homeList.indexOf(item), 1);
      this.homeList = [...this.homeList];

    }
  },

  watch: {
    homeList(newValue) {
      localStorage.homeList = newValue;
    }
  },

  mounted() {
    if (localStorage.homeList) {
      this.homeList = localStorage.homeList.split(",");
    } else {
      let defaultValue = ["https://www.airbnb.com/rooms/16420029"];
      this.homeList = defaultValue;
    }

    this.orderAscending = true;
    this.checkinDate = moment()
      .startOf("day")
      .add(1, "days")
      .format("YYYY-MM-DD");
    this.checkoutDate = moment()
      .startOf("day")
      .add(15, "days")
      .format("YYYY-MM-DD");
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
    padding-bottom: 5vh;

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
          // border: 1px solid #ddd;
          // border-collapse: none;
          padding: 5px 3px 5px 5px;
          line-height: 25px;
          text-align: left;
          min-width: 60px;
        }

        .failure {
          color: white;
          background: #ff5a5f;
          text-decoration: line-through;
        }

        .success {
          background: #1c797b;
          color: white;
        }
      }

      .v-data-footer {
        display: none !important;
      }
    }
  }
}
</style>
