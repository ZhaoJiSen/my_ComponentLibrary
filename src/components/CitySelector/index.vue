<template>
  <div>
    <union-selector
        :is-show="isProvinceSelectorShow"
        default-option="请选择 - 省"
        code-key="provinceCode"
        :data="provinces"
        @handle-change="handleProvinceChange"
    ></union-selector>
    <union-selector
        :is-show="isCitySelectorShow"
        default-option="请选择 - 市"
        code-key="cityCode"
        :data="state.cities"
        @handle-change="handleCityChange"
    ></union-selector>
    <union-selector
        :is-show="isCountySelectorShow"
        default-option="请选择 - 区"
        code-key="countyCode"
        :data="state.counties"
        @handle-change="handleCountyChange"
    ></union-selector>
    <!--    <select class="city-selector" v-if="isProvinceSelectorShow" @change="handleProvinceChange">-->
    <!--      <option value="">请选择 - 省</option>-->
    <!--      <option :value="`${ info.provinceCode }:${ info.provinceName }`" v-for="(info, index) in provinces" :key="index">-->
    <!--        {{ info.provinceName }}-->
    <!--      </option>-->
    <!--    </select>-->
    <!--    <select class="city-selector" v-if="isCitySelectorShow" @change="handleCityChange">-->
    <!--      <option value="">请选择 - 市</option>-->
    <!--      <option :value="`${ city.cityCode }:${ city.cityName }`" v-for="(city, index) in state.cities" :key="index">-->
    <!--        {{ city.cityName }}-->
    <!--      </option>-->
    <!--    </select>-->
    <!--    <select class="city-selector" v-if="isCountySelectorShow" @change="handleCountyChange">-->
    <!--      <option value="">请选择 - 市</option>-->
    <!--      <option :value="`${ county.countyCode }:${ county.countyName }`" v-for="(county, index) in state.counties"-->
    <!--              :key="index">-->
    <!--        {{ county.countyName }}-->
    <!--      </option>-->
    <!--    </select>-->
  </div>
</template>

<script setup>
import {reactive, computed} from "vue";
import cityData from "./city";
import UnionSelector from "./components/UnionSelector.vue";

const state = reactive({
  cities: null,
  counties: null,
  selectedInfo: {
    province: null,
    cities: null,
    counties: null,
  }
})

const isProvinceSelectorShow = computed(() => !!provinces);
const isCitySelectorShow = computed(() => !!state.cities);
const isCountySelectorShow = computed(() => !!state.counties)

const formatData = (data) => {

  return data.reduce((prev1, next1) => {

    next1.cities = next1.cities.reduce((prev2, next2) => {

      next2.counties = next2.counties.reduce((prev3, next3) => {
        prev3[next3.countyName] = next3;
        return prev3;
      }, {})

      prev2[next2.cityName] = next2;
      return prev2;
    }, {})

    prev1[next1.provinceName] = next1;
    return prev1;
  }, {})
}
const handleProvinceChange = (value) => {

  if (!value) {
    state.selectedInfo.province = null;
    state.selectedInfo.cities = null;
    state.selectedInfo.counties = null;
    state.cities = null;
    state.counties = null;
    return;
  }

  const [code, name] = value.split(":");
  console.log(code, name)
  state.selectedInfo.province = {
    code,
    name
  }
  state.cities = provinces[name].cities;
}
const handleCityChange = (value) => {
  if (!value) {
    state.selectedInfo.cities = null;
    state.selectedInfo.counties = null;
    state.counties = null;
    return;
  }
  const [code, name] = value.split(":");
  state.selectedInfo.city = {
    code,
    name
  }
  state.counties = state.cities[name].counties;
}
const handleCountyChange = (value) => {
  if (!value) {

    return;
  }
  const [code, name] = value;
  state.selectedInfo.counties = {
    name,
    code
  }
}
const provinces = formatData(cityData);
</script>