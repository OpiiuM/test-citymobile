<template>
  <div class="content">
    <form class="content__form form">
      <input type="text" class="form__input" placeholder="Поиск" v-model="search" />
      <button class="form__btn btn" type="submit" @click.prevent="">Найти</button>
    </form>
    <div class="content__wrapper">
      <table class="content__table table">
        <thead>
          <tr class="table__row">
            <th :style="{display: 'flex', 'justify-content': 'space-between',}"
              >Марка и модель
              <button
                @click="changeFilter"
                :style="{color: '#000', border: 'none', background: 'none',}"
              >{{ filter[filterStatus] }}</button>
            </th>
            <th v-for="(cat, i) in categories" :key="i">{{ cat }}</th>
          </tr>
        </thead>
        <tbody>
          <tr 
            class="table__row" 
            :style="{cursor: 'pointer'}" 
            v-for="(info, i) in searchedData" 
            :key="i" 
            @click="getRow($event)"
          >
            <td>{{ info.mark }} {{ info.model }}</td>
            <td
              v-for="zxc, index of categories"
              :key="index"
              @click="getCell(index)"
            >
              {{ info.tariffs?.[categories[index]]?.year ?? "-" }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div class="content__select select">
      <p class="select__text">Выбран автомобиль {{ selectItem }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "content",
  data() {
    return {
      selectItem: "...",
      filterStatus: false,
      filter: { false: "▼", true: "▲" },

      infoCars: [],
      categories: [],

      cellIndex: 0,
      carMark: "",

      search: "",
    };
  },
  methods: {
    changeFilter() {
      this.infoCars.reverse()
      this.filterStatus = !this.filterStatus;
    },
    
    getRow(event) {
      this.carMark = event.target.parentNode.children[0].innerText
      const year = event.target.parentNode.children[this.cellIndex + 1].innerText

      if (year !== "-") {
        this.selectItem = `${this.carMark} ${year} года выпуска`
      }
    },

    getCell(idx) {
      this.cellIndex = idx
    },

    submitHandler() {

    }
  },
  computed: {
    sortedData() {
      return this.infoCars
    },

    searchedData() {
      return this.sortedData.filter(info => `${info.mark} ${info.model}`.toLowerCase().includes(this.search.toLowerCase()))
    }
  },
  async mounted() {
    try {
      const response = await axios.get("https://city-mobil.ru/api/cars");
      this.infoCars = response.data["cars"];
      this.categories = response.data["tariffs_list"];
    } catch (e) {
      console.log(e);
    }
  },
};
</script>