<template src="./Dropdown.template.html">
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component';
import axios, { AxiosResponse } from 'axios';
import { v4 as uuidv4 } from 'uuid';

import Spinner from '../Spinner.vue';
import Cities from './Cities/Cities.vue';
import ICountry from './interfaces/country.interface';
import IState from './interfaces/state.interface';
import ICity from './interfaces/city.interface';

@Options({
  components: {
    Spinner,
    Cities,
  },
})
export default class HelloWorld extends Vue {
  loading = false;
  dropdownOpen = false;
  token = "";
  countries: ICountry[] = [];
  states: IState[] = [];
  cities: ICity[] = [];
  selectedCountry: ICountry | null = null;
  selectedState: IState | null = null;
  selectedCities: ICity[] = [];

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

  async fetchStates(countryName: string): Promise<void> {
    try {
      this.loading = true;
      const res: AxiosResponse = await axios.get(`https://www.universal-tutorial.com/api/states/${countryName}`, {
        headers: {
          "Authorization": `Bearer ${this.token}`,
          "Accept": "application/json"
        }
      });
      this.states = res.data.map((state: IState) => ({
        id: uuidv4(),
        ...state,
      }));
      this.loading = false;
    }
    catch (error) {
      console.log(error);
    }
  }

  async fetchCities(cityName: string): Promise<void> {
    try {
      this.loading = true;
      const res: AxiosResponse = await axios.get(`https://www.universal-tutorial.com/api/cities/${cityName}`, {
        headers: {
          "Authorization": `Bearer ${this.token}`,
          "Accept": "application/json"
        }
      });
      
      this.cities = res.data.map((city: ICity) => ({
        id: uuidv4(),
        ...city,
      }));
      this.loading = false;
    }
    catch (error) {
      console.log(error);
    }
  }

  changeDropdownState(): void {
    this.dropdownOpen = !this.dropdownOpen;
  }

  clear(): void {
    this.selectedState = null;
    this.selectedCountry = null;
    this.states = [];
    this.dropdownOpen = false;
  }

  getDropdownHeaderLabel(): string {
    const countryName = this.selectedCountry?.country_name;
    const stateName = this.selectedState?.state_name;
    if (countryName && stateName) {
      return `${countryName}, ${stateName}`;
    }
    else if (countryName) {
      return countryName;
    }
    return "Select country";
  }

  getIsCountrySelected(country: ICountry): boolean {
    return this.selectedCountry?.id === country.id;
  }
  
  selectCountry(country: ICountry): void {
    this.selectedCountry = country;
    this.fetchStates(country?.country_name || "");
  }

  selectState(state: IState): void {
    this.dropdownOpen = false;
    this.selectedState = state;
    this.fetchCities(state.state_name || "");
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
<style scoped src="./Dropdown.style.scss" lang="scss" />
