<template>
  <div
    :class="{
      'card': true,
      'card--selected': props.data.selected
    }"
  >
    <img class="card__img" :src="props.data.picture" />
    <div class="info">
      <div>
        <Highlighter class="info__name"
          highlightClassName="highlight"
          :searchWords="[props.highlight]"
          :autoEscape="true"
          :textToHighlight="`${props.data.name.first} ${props.data.name.last}`"
        />
        <Highlighter class="info__email"
          highlightClassName="highlight"
          :searchWords="[props.highlight]"
          :autoEscape="true"
          :textToHighlight="props.data.email"
        />
      </div>
      <div class="detail">
        <div class="detail__item">{{ `Registered: ${props.data.registered}` }}</div>
        <Highlighter class="detail__item"
          highlightClassName="highlight"
          :searchWords="[props.highlight]"
          :autoEscape="true"
          :textToHighlight="props.data.address"
        />
        <Highlighter class="detail__item"
          highlightClassName="highlight"
          :searchWords="[props.highlight]"
          :autoEscape="true"
          :textToHighlight="props.data.country"
        />
      </div>
      <div class="divider" />
      <div class="button" @click="() => props.onSelect(props.data.email)">{{ props.data.selected ? 'UNSELECT' : 'SELECT' }}</div>
    </div>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'
import Highlighter from 'vue-highlight-words'

const props = defineProps({
  data: Object,
  onSelect: Function,
  highlight: String
})
</script>

<style lang="scss" scoped>
.card {
  display: flex;
  align-items: flex-start;
  padding: 0px;
  gap: 16px;
  width: 100%;
  background: #F9F9F9;
  box-shadow: 4px 4px 5px rgba(0, 0, 0, 0.1);
  border-radius: 8px;
  border: 1px solid transparent;

  &__img {
    width: 179px;
    height: 167.11px;
    border-radius: 8px 0px 0px 8px;
  }

  &--selected {
    border: 1px solid #6B81F3;
  }
}

.info {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-end;
  padding: 16px;
  gap: 8px;
  flex-grow: 1;

  div:first-child {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0px;
    gap: 32px;
    width: 100%;
  }

  &__name {
    font-weight: 700;
    font-size: 21px;
    line-height: 25px;
    color: #333333;
  }

  &__email {
    font-weight: 400;
    font-size: 14px;
    line-height: 17px;
    color: #333333;
  }
}

.detail {
  display: flex;
  flex-direction: column;
  gap: 4px;
  width: 100%;

  &__item {
    font-weight: 400;
    font-size: 16px;
    line-height: 19px;
    color: #333333;
  }
}

.divider {
  width: 100%;
  border: 1px solid #EFEFEF;
}

.button {
  font-weight: 700;
  font-size: 16px;
  line-height: 19px;
  color: #6B81F3;
  cursor: pointer;
}
</style>
