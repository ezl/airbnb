<template>

  <div class="loginPage">
    <div class="innerPart">
      <div class="box">
        <!-- Header -->
        <h1 class="header">Find places to stay in India on Airbnb</h1>

        <div class="description">Discover entire homes and private rooms perfect for any trip.</div>

        <!-- Form -->
        <form id="detailsForm">
          <!-- Where -->
          <div>
            <div class="where">WHERE</div>
            <input type="text" autocomplete="off" aria-autocomplete="list" aria-expanded="false" aria-haspopup="true" class="anyWhere" name="location" placeholder="Anywhere" role="combobox" value="">

          </div>

          <!-- Check In - OUT -->
          <div style="display:flex">
            <div style="flex:3">
              <div class="checkIn">CHECK-IN</div>
              <div> <input class="inputCheck" data-veloute="checkin_input" id="checkin_input" name="checkin" placeholder="dd-mm-yyyy" type="text" value="" readonly=""></div>
            </div>
            <div style="flex:3">
              <div class="checkOut">CHECK-OUT</div>
              <div> <input class="inputCheck" data-veloute="checkin_input" id="checkin_input" name="checkin" placeholder="dd-mm-yyyy" type="text" value="" readonly=""></div>
            </div>
          </div>

          <!--Select Adult - Child  -->
          <div style="display:flex">
            <div style="flex:3">
              <div class="adults">ADULTS</div>
              <div>
                <select name="adults" class="selectBox" data-vars-select-name="adults">
                  <option selected="" value="1">1 adult</option>
                  <option value="2">2 adults</option>
                  <option value="3">3 adults</option>
                  <option value="4">4 adults</option>
                  <option value="5">5 adults</option>
                  <option value="6">6 adults</option>
                  <option value="7">7 adults</option>
                  <option value="8">8 adults</option>
                  <option value="9">9 adults</option>
                  <option value="10">10 adults</option>
                  <option value="11">11 adults</option>
                  <option value="12">12 adults</option>
                  <option value="13">13 adults</option>
                  <option value="14">14 adults</option>
                  <option value="15">15 adults</option>
                  <option value="16">16 adults</option>
                </select>

              </div>
            </div>
            <div style="flex:3">
              <div class="children">CHILDREN</div>
              <div>
                <select name="children" class="selectBox" data-vars-select-name="children">
                  <option selected="" value="0">0 children</option>
                  <option value="1">1 child</option>
                  <option value="2">2 children</option>
                  <option value="3">3 children</option>
                  <option value="4">4 children</option>
                  <option value="5">5 children</option>
                </select>
              </div>
            </div>
          </div>

          <!-- Button -->
          <button type="submit" class="btn" aria-busy="false">Search</button>

        </form>
      </div>
    </div>
  </div>

</template>
<style >
body {
  margin: 0px !important;
  height: 100vh;
  overflow: hidden;
}
.loginPage {
  background-image: url("https://stmed.net/sites/default/files/gold-coast-wallpapers-28606-2118026.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center center;
  height: 650px;
}
.innerPart {
  padding: 100px 24px 0 130px;
  max-width: 1080px !important;
  background-color: transparent;
}
.box {
  background: #ffffff !important;
  border-radius: 4px !important;
  width: 430px !important;
  margin: 0px 0 64px 40px;
  padding: 32px !important;
}
.header {
  color: inherit !important;
  font-size: 30px !important;
  font-family: Circular, -apple-system, BlinkMacSystemFont, Roboto,
    Helvetica Neue, sans-serif !important;
  margin: 0px !important;
  padding: 0px !important;
  line-height: 36px !important;
}
.description {
  font-size: 18px !important;
  line-height: 26px !important;
  letter-spacing: normal !important;
  font-family: Circular, -apple-system, BlinkMacSystemFont, Roboto,
    Helvetica Neue, sans-serif !important;
  color: #484848 !important;
  margin: 8px 0 !important;
}
#detailsForm {
}
.where {
  margin: 18px 0 8px 0;
  overflow-wrap: break-word !important;
  font-family: Circular, -apple-system, BlinkMacSystemFont, Roboto,
    "Helvetica Neue", sans-serif !important;
  font-size: 12px !important;
  font-weight: 700 !important;
  line-height: 1.33333em !important;
  letter-spacing: 0.0833333em !important;
  color: rgb(72, 72, 72) !important;
}
.anyWhere {
  font-size: var(--font-form-element-font-size, 16px) !important;
  line-height: var(--font-form-element-line-height, 24px) !important;
  letter-spacing: var(--font-form-element-letter-spacing, normal) !important;
  font-family: var(
    --font-form-element-font-family,
    Circular,
    -apple-system,
    BlinkMacSystemFont,
    Roboto,
    Helvetica Neue,
    sans-serif
  ) !important;
  color: var(--font-form-element-color, #484848) !important;
  padding-top: var(--spacing-form-element-vertical, 7px) !important;
  padding-bottom: var(--spacing-form-element-vertical, 7px) !important;
  padding-left: var(--spacing-form-element-horizontal, 11px) !important;
  padding-right: var(--spacing-form-element-horizontal, 11px) !important;
  font-weight: var(--font-book-font-weight, 600) !important;
  display: inline-block;
  position: relative;
  width: 94%;
}
.checkIn,
.checkOut,
.adults,
.children {
  overflow-wrap: break-word !important;
  font-family: Circular, -apple-system, BlinkMacSystemFont, Roboto,
    "Helvetica Neue", sans-serif !important;
  font-size: 12px !important;
  font-weight: 700 !important;
  line-height: 1.33333em !important;
  letter-spacing: 0.0833333em !important;
  color: rgb(72, 72, 72) !important;
  margin: 23px 0 8px 0;
}
.inputCheck {
  color: #484848 !important;
  font-weight: 600 !important;
  font-size: 16px !important;
  width: 93%;
  padding-top: var(--spacing-form-element-vertical, 11px) !important;
  padding-bottom: var(--spacing-form-element-vertical, 11px) !important;
  padding-left: var(--spacing-form-element-horizontal, 11px) !important;
}
.selectBox {
  font-size: var(--font-form-element-font-size, 16px) !important;
  line-height: var(--font-form-element-line-height, 24px) !important;
  letter-spacing: var(--font-form-element-letter-spacing, normal) !important;
  font-family: var(
    --font-form-element-font-family,
    Circular,
    -apple-system,
    BlinkMacSystemFont,
    Roboto,
    Helvetica Neue,
    sans-serif
  ) !important;
  width: 100%;
  text-transform: var(--font-form-element-text-transform, undefined) !important;
  color: var(--font-form-element-color, #484848) !important;
  padding-top: var(--spacing-form-element-vertical, 11px) !important;
  padding-bottom: var(--spacing-form-element-vertical, 11px) !important;
  font-weight: var(--font-light-font-weight, normal) !important;
  background-color: var(--color-clear, transparent) !important;
  padding-left: var(--spacing-form-element-horizontal, 11px) !important;
  padding-right: var(--spacing-select-arrow, 40px) !important;
}
.btn {
  border-radius: var(--border-button-border-radius, 4px) !important;
  font-size: var(--font-button-font-size, 16px) !important;
  line-height: var(--font-button-line-height, 24px) !important;
  letter-spacing: var(--font-button-letter-spacing, normal) !important;
  font-family: var(
    --font-button-font-family,
    Circular,
    -apple-system,
    BlinkMacSystemFont,
    Roboto,
    Helvetica Neue,
    sans-serif
  ) !important;
  padding-top: var(--spacing-button-vertical, 7px) !important;
  padding-bottom: var(--spacing-button-vertical, 7px) !important;
  font-weight: var(--font-bold-font-weight, 700) !important;
  border-width: var(--border-button-border-width, 2px) !important;
  padding-left: var(--spacing-button-horizontal, 22px) !important;
  padding-right: var(--spacing-button-horizontal, 22px) !important;
  background: var(--color-buttons-primary-color, #ff5a5f) !important;
  border-color: var(--color-buttons-primary-border, transparent) !important;
  color: var(--color-buttons-primary-text, #ffffff) !important;
  width: 100%;
  margin-top: 15px;
}
</style>