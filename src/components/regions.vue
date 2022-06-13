<template>
  <div class="regions">
    <div class="box">
      <p>Регион</p>
      <div class="loadingCircle">
        <select v-model="selectedRegion" class="selectOptions">
          <option value="" disabled selected hidden>Выберите регион</option>
          <option
            v-for="regionsList in regionsList"
            :value="regionsList.id"
            :key="regionsList.id"
          >
            {{ regionsList.title }}
          </option>
        </select>
      </div>

      <p>Район</p>
      <div class="loadingCircle">
        <select v-model="selectedDistrict" class="selectOptions">
          <option value="" disabled selected hidden>Выберите район</option>
          <option
            v-for="districtsList in districtsList"
            :value="districtsList.id"
            :key="districtsList.id"
          >
            {{ districtsList.title }}
          </option>
        </select>
      </div>

      <p>Учреждение</p>
      <div class="loadingCircle">
        <select v-model="selectedOrganization" class="selectOptions">
          <option value="" disabled selected hidden>Выберите учреждение</option>
          <option
            v-for="organizationsList in organizationsList"
            :value="organizationsList.id"
            :key="organizationsList.id"
          >
            {{ organizationsList.title }}
          </option>
        </select>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "regions",
  data() {
    return {
      selectedRegion: "",
      regionsList: [],

      selectedDistrict: "",
      districtsList: [],

      selectedOrganization: "",
      organizationsList: [],
    };
  },
  mounted() {
    this.getRegions();
  },
  methods: {
    getRegions() {
      fetch(`http://my2.dev.gov.uz/azamat/ru/weather/get-regions`)
        .then((response) => response.json())
        .then((data) => {
          this.regionsList = data;
        });
    },
    getDistricts() {
      fetch(
        `http://my2.dev.gov.uz/azamat/ru/weather/get-districts?regionId=${this.selectedRegion}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.districtsList = data;
          console.log(this.optionsOfRegions);
        });
    },
    getOrganizations() {
      fetch(
        `http://my2.dev.gov.uz/azamat/ru/weather/get-organizations?districtId=${this.selectedDistrict}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.organizationsList = data;
          console.log(this.optionsOfCompanys);
        });
    },
  },
  watch: {
    selectedRegion: function () {
      this.selectedDistrict = "";
      this.selectedOrganization = "";

      this.getDistricts();
    },
    selectedDistrict: function () {
      this.getOrganizations();
    },
  },
};
</script>

<style scoped>
.regions {
  width: 100%;
  height: 100vh;
  display: grid;
  place-items: center;
  background-color: #0f4d0f;
}
.box {
  width: 500px;
  height: 600px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  color: #fff;
}
.box p {
  width: 100%;
  text-align: left;
  padding: 0 0 10px 30px;
}
.selectOptions {
  width: 100%;
  padding: 15px 10px;
  margin-bottom: 70px;
  background: #1d781d;
  outline: none;
  border: 1px solid #288c28;
  border-radius: 30px;
  color: #fff;
  appearance: none;
  position: relative;
}

.loadingCircle {
  width: 100%;
  position: relative;
}
</style>
