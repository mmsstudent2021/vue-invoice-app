<template>
  <tr>
    <td>
      <span class="show-in-print">{{ record.id }}</span>
      <i @click="$emit('toDel',record.id)" class="fa-solid fa-trash-alt text-danger hide-in-print"></i>
    </td>
    <td>{{ record.service.name }}</td>
    <td class="text-end">{{ record.service.price }}</td>
    <td class="text-end quantity-width" @dblclick="quantityEdit=true">
      <input v-if="quantityEdit" v-model="record.quantity" @keyup.enter="exitQuantityEdit" @change="updateQuantity" type="number" min="1" class="form-control">
      <span v-else>{{ record.quantity }}</span>
    </td>
    <td class="text-end">{{ record.cost }}</td>
  </tr>
</template>

<script>
export default {
  props: {
    record: Object,
  },
  data() {
    return {
      quantityEdit: false,

    }
  },
  methods: {
    exitQuantityEdit(){
      this.quantityEdit = false;
    },
    updateQuantity() {
      this.record.cost = this.record.service.price * this.record.quantity
    }
  },
}
</script>

<style scoped>
  .quantity-width{
    width: 150px;
  }
</style>