<template>
  <div :class="$style['goods-card']" >
    <div
      :class="[$style['good-card'], $style[effectWhenRemoved === good.id ? 'effectremove' : '']]"
      v-for="good in filterGoodsList"
      :key="good"
    >
      <img
        :src="good.urlImg"
        alt="фотография товара"
        :class="$style['good-card__img']"
      />
      <div :class="$style['good-card__wrapp']">
        <h3 :class="['fs-20', $style['good-card__title']]">
          {{ good.name }}
        </h3>
        <p :class="['fs-16', $style['good-card__description']]">
          {{ good.description }}
        </p>
        <p :class="[$style['good-card__price'], 'fs-24']">
          {{ good.price.toLocaleString()
          }}<span class="$style['good-card__price-currency']"> руб.</span>
        </p>
      </div>
      <svg
        @click="removeGood(good)"
        :class="$style['good-card__cart']"
        width="40"
        height="40"
        viewBox="0 0 40 40"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
        :svgId="good.id"
      >
        <rect x="4" y="2" width="32" height="32" rx="10" fill="#FF8484" />
        <path
          d="M22.207 15.7969C22 15.7969 21.8322 15.9646 21.8322 16.1716V23.2535C21.8322 23.4604 22 23.6283 22.207 23.6283C22.4139 23.6283 22.5817 23.4604 22.5817 23.2535V16.1716C22.5817 15.9646 22.4139 15.7969 22.207 15.7969Z"
          fill="white"
        />
        <path
          d="M17.7854 15.7969C17.5785 15.7969 17.4107 15.9646 17.4107 16.1716V23.2535C17.4107 23.4604 17.5785 23.6283 17.7854 23.6283C17.9924 23.6283 18.1602 23.4604 18.1602 23.2535V16.1716C18.1602 15.9646 17.9924 15.7969 17.7854 15.7969Z"
          fill="white"
        />
        <path
          d="M14.5629 14.7634V23.9953C14.5629 24.541 14.763 25.0534 15.1126 25.4211C15.4605 25.7898 15.9447 25.9991 16.4514 26H23.541C24.0478 25.9991 24.532 25.7898 24.8798 25.4211C25.2293 25.0534 25.4294 24.541 25.4294 23.9953V14.7634C26.1242 14.5789 26.5745 13.9077 26.4815 13.1947C26.3884 12.4819 25.7811 11.9487 25.0622 11.9485H23.1437V11.4801C23.1459 11.0863 22.9902 10.708 22.7113 10.4298C22.4325 10.1517 22.0537 9.99683 21.6598 10H18.3325C17.9387 9.99683 17.5599 10.1517 17.281 10.4298C17.0022 10.708 16.8465 11.0863 16.8487 11.4801V11.9485H14.9302C14.2112 11.9487 13.6039 12.4819 13.5108 13.1947C13.4179 13.9077 13.8681 14.5789 14.5629 14.7634V14.7634ZM23.541 25.2506H16.4514C15.8107 25.2506 15.3124 24.7002 15.3124 23.9953V14.7963H24.68V23.9953C24.68 24.7002 24.1816 25.2506 23.541 25.2506ZM17.5981 11.4801C17.5956 11.285 17.6723 11.0972 17.8107 10.9595C17.9491 10.8218 18.1373 10.7461 18.3325 10.7495H21.6598C21.8551 10.7461 22.0433 10.8218 22.1816 10.9595C22.3201 11.0971 22.3968 11.285 22.3943 11.4801V11.9485H17.5981V11.4801ZM14.9302 12.6979H25.0622C25.4347 12.6979 25.7367 12.9999 25.7367 13.3724C25.7367 13.7449 25.4347 14.0469 25.0622 14.0469H14.9302C14.5577 14.0469 14.2557 13.7449 14.2557 13.3724C14.2557 12.9999 14.5577 12.6979 14.9302 12.6979V12.6979Z"
          fill="white"
        />
        <path
          d="M19.9962 15.7969C19.7892 15.7969 19.6215 15.9646 19.6215 16.1716V23.2535C19.6215 23.4604 19.7892 23.6283 19.9962 23.6283C20.2032 23.6283 20.3709 23.4604 20.3709 23.2535V16.1716C20.3709 15.9646 20.2032 15.7969 19.9962 15.7969Z"
          fill="white"
        />
      </svg>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapMutations } from "vuex";

export default {
  name: "GoodsCard",
  data() {
    return {
      sortedGoods: [],
      effectWhenRemoved: undefined,
    };
  },
  methods: {
    ...mapMutations(["removalGood", "sortGoodsList"]),
    removeGood(good) {
      this.effectWhenRemoved = good.id;
      setTimeout(() => {
        this.removalGood(good);
      }, 300);
    },
  },
  computed: {
    ...mapGetters(["getGoodsList", "getSelected"]),
    filterGoodsList() {
      /* eslint-disable */
      this.sortedGoods = [...this.getGoodsList];
      switch (this.getSelected) {
        case "Цена по возрастанию":
          return this.sortedGoods.sort(function(a, b) {
            return a.price - b.price;
          });
        case "Цена по убыванию":
          return this.sortedGoods.sort(function(a, b) {
            return b.price - a.price;
          });
        case "По названию":
          return this.sortedGoods.sort((a, b) => a.name.localeCompare(b.name));
        default:
          return this.sortedGoods;
      }
    }
  }
};
</script>

<style lang="scss" module>
.goods-card {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 16px;
  @media (max-width: 1405px) {
    justify-content: space-evenly;
  }

  &:after {
    content: " i i ";
    word-spacing: 332px;
    margin-left: -24px;
    opacity: 0;
  }
}

.good-card {
  display: grid;
  grid-template-rows: 200px 1fr;
  grid-template-columns: 320px;
  align-items: stretch;
  background: $bg;
  box-shadow: 0 20px 30px rgba(0, 0, 0, 0.04), 0 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
  position: relative;
  transition: 0.4s ease-out;
  cursor: pointer;
  position: relative;

  &__wrapp {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    gap: 16px;
    padding: 16px 16px 24px 16px;
  }

  &__img {
    width: 100%;
    object-fit: cover;
    background: $btn-dis;
    -moz-user-select: none;
    -o-user-select:none;
    -khtml-user-select: none;
    -webkit-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  &__title,
  &__description {
    word-break: break-word;
    hyphens: auto;
  }

  &__price {
    margin-top: 16px;
  }

  &__cart {
    position: absolute;
    top: -10px;
    right: -12px;
    opacity: 0;

    rect, path {
      pointer-events: none;
    }

    &:hover {
      transform: scale(1.2);
    }
  }

  &:hover &__cart {
    opacity: 1;
    transition: 0.4s ease-out;
  }

  &:hover {
    box-shadow: 0 20px 30px rgba(0, 0, 0, 0.16), 0 6px 10px rgba(0, 0, 0, 0.02);
    transition: 0.2s ease-out;
  }
}
.effectremove {
  opacity: 0;
  transition: 0.2s ease-out;
}
</style>
