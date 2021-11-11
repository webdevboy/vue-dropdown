<template src="./Cities.template.html">
</template>

<script lang="ts">
import { Options, Vue, prop } from 'vue-class-component';
import ICity from '../interfaces/city.interface';

class Props {
  cities = prop<ICity[]>({ default: [] })
}

@Options({
  watch: {
    cities(): void {
      this.selectedCities = [];
    }
  }
})
export default class Cities extends Vue.with(Props) {
  selectedCities: ICity[] = [];

  getIsCitySelected(city: ICity): boolean {
    return !!this.selectedCities.find(item => item.id === city.id);
  }

  selectCity(city: ICity): void {
    const isSelected = this.getIsCitySelected(city);
    if (isSelected) {
      this.selectedCities = this.selectedCities.filter((selectedCity): boolean =>
        selectedCity.id !== city.id,
      );
    }
    else {
      this.selectedCities = [...this.selectedCities, city];
    }
  }
}
</script>

<style lang="scss" scoped src="./Cities.style.scss">
</style>
