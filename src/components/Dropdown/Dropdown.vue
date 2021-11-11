<template>
  <div class="dropdown-container">
    <div
      v-on:click="changeDropdownState"
      v-bind:class="{ dropdown__head__disabled: loading, dropdown__head__open: dropdownOpen }"
      class="dropdown__head"
    >
      {{getDropdownHeaderLabel()}}
      <div v-if="loading" class="dropdown__head__loader">
        <Spinner />
      </div>
    </div>
    <div v-if="dropdownOpen" class="dropdown__body">
      <div class="dropdown__body__buttons">
        <button v-on:click="selectAll">Select All</button>
        <button v-on:click="deselectAll">Deselect All</button>
      </div>
      <div class="dropdown__body__scroll">
        <div
          class="dropdown__body__item"
          v-for="(country) of countries"
          v-bind:class="{ dropdown__body__item__selected: getIsCountrySelected(country) }"
          v-on:click="selectCountry(country)"
          :key="country.id"
        >
          {{country.country_name}}
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import axios, { AxiosResponse } from 'axios';
import { v4 as uuidv4 } from 'uuid';

import Spinner from '../Spinner.vue';
import ICountry from './interfaces/country.interface';

@Options({
  components: {
    Spinner,
  },
})
export default class HelloWorld extends Vue {
  loading = false;
  dropdownOpen = false;
  token = "";
  countries: ICountry[] = [];
  selectedCountries: ICountry[] = [];

  changeDropdownState(): void {
    this.dropdownOpen = !this.dropdownOpen;
  }

  getDropdownHeaderLabel(): string {
    if (this.selectedCountries.length > 0) {
      const countryNames = this.selectedCountries.map(
        (country): string | undefined => country.country_name,
      );
      return countryNames.join(", ");
    }
    return "Select country";
  }

  getIsCountrySelected(country: ICountry): boolean {
    return !!this.selectedCountries.find((item): boolean => item.id === country.id);
  }
  
  selectCountry(country: ICountry): void {
    const isCountrySelected = this.getIsCountrySelected(country);
    if (isCountrySelected) {
      this.selectedCountries = this.selectedCountries.filter(
        (item): boolean => item.id !== country.id,
      );
    }
    else {
      this.selectedCountries = [...this.selectedCountries, country];
    }
  }

  selectAll(): void {
    this.selectedCountries = this.countries;
  }

  deselectAll(): void {
    this.selectedCountries = [];
  }

  async fetchCountries(): Promise<void> {
    try {
      const res: AxiosResponse = await axios.get("https://www.universal-tutorial.com/api/countries/", {
        headers: {
          "Authorization": `Bearer ${this.token}`,
          "Accept": "application/json"
        }
      });
      this.countries = res.data.map((country: ICountry) => ({
        id: uuidv4(),
        ...country,
      }));
      this.loading = false;
    }
    catch (error) {
      console.log(error);
      this.loading = false;
    }
  }

  async mounted(): Promise<void> {
    try {
      this.loading = true;
      // Fetch api token and countries on mount
      const tokenRes: AxiosResponse = await axios.get("https://www.universal-tutorial.com/api/getaccesstoken", {
        headers: {
          "Accept": "application/json",
          "api-token": "FcXAVZSNbVvgIy9Bq76L4zxIdlC0BNV0hUZRMuZ3DiZBeNkv7JKpB-57Ui58F6wrRJ8",
          "user-email": "prihe89@gmail.com"
        }
      });
      this.token = tokenRes.data.auth_token;
      this.fetchCountries();
    }
    catch (error) {
      console.log(error);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
  .dropdown-container {
    min-height: 40px;
    width: 400px;
    margin: 0 auto;
    position: relative;
    .dropdown__head {
      min-height: 40px;
      text-align: left;
      padding: 10px 50px 10px 15px;
      box-sizing: border-box;
      display: flex;
      justify-content: flex-start;
      align-items: center;
      border: 1px solid rgba(0, 0, 0, 0.4);
      cursor: pointer;
      border-radius: 5px;
      position: relative;
      box-shadow: 0 0 2px 0 #cbcbcb;
      .dropdown__head__loader {
        position: absolute;
        top: 50%;
        right: 20px;
        transform: translateY(-50%);
        position: absolute;
      }
    }
    .dropdown__head__open {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
    }
    .dropdown__head__disabled {
      opacity: 0.4;
      pointer-events: none;
    }
    .dropdown__body {
      height: 300px;
      width: calc(100% - 2px);
      position: absolute;
      bottom: -300px;
      border: 1px solid rgba(0, 0, 0, 0.4);
      border-top: none;
      background-color: white;
      z-index: 10;
      border-bottom-left-radius: 5px;
      border-bottom-right-radius: 5px;
      box-shadow: 0 3px 5px 0 #cbcbcb;
      .dropdown__body__buttons {
        height: 50px;
        display: flex;
        justify-content: space-around;
        align-items: center;
        box-shadow: 0 3px 5px 0 #cbcbcb;
        button {
          font-size: 16px;
          border: none;
          background-color: #a0b2ff;
          padding: 8px 20px;
          border-radius: 3px;
          cursor: pointer;
        }
      }
      .dropdown__body__scroll {
        height: 250px;
        overflow-y: auto;
        width: 100%;
        .dropdown__body__item {
          text-align: left;
          padding: 10px 20px;
          cursor: pointer;
          transition: all 0.2s ease;
          &:hover {
            background-color: #e9e9e9;
          }
        }
        .dropdown__body__item__selected {
          background-color: #cbcbcb;
          &:hover {
            background-color: #cbcbcb;
          }
        }
      }
    }
  }
</style>
