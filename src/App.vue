<template>
  <div class="container-sm">
    <div class="card m-1">
      <div class="card-header">Крайна цена</div>
      <div class="card-body">
        <div class="row">
          <div class="col mb-2">
            <div class="row g-3 align-items-center">
              <div class="col-auto">
                <label for="end-price-euro" class="form-label mr-1">Евро</label>
              </div>
              <div class="col-auto">
                <input
                  id="end-price-euro"
                  type="text"
                  class="form-control-sm"
                  v-model="endPriceEuro"
                  @focus="setFocus('end-price-euro')"
                />
              </div>
            </div>
          </div>
          <div class="col">
            <div class="row g-3 align-items-center">
              <div class="col-auto">
                <label for="end-price-lv" class="form-label mr-1">Лева</label>
              </div>
              <div class="col-auto">
                <input
                  id="end-price-lv"
                  type="text"
                  class="form-control-sm"
                  v-model="endPriceLv"
                  @focus="setFocus('end-price-lv')"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="card m-1">
      <div class="card-header">Платена сума</div>
      <div class="card-body">
        <div class="row">
          <div class="col mb-2">
            <div class="row g-3 align-items-center">
              <div class="col-auto">
                <label for="amount-paid-euro" class="form-label mr-1">Евро</label>
              </div>
              <div class="col-auto">
                <input
                  id="amount-paid-euro"
                  type="text"
                  class="form-control-sm"
                  v-model="amountPaidEuro"
                  @focus="setFocus('amount-paid-euro')"
                />
              </div>
            </div>
          </div>
          <div class="col">
            <div class="row g-3 align-items-center">
              <div class="col-auto">
                <label for="amount-paid-lv" class="form-label mr-1">Лева</label>
              </div>
              <div class="col-auto">
                <input
                  id="amount-paid-lv"
                  type="text"
                  class="form-control-sm"
                  v-model="amountPaidLv"
                  @focus="setFocus('amount-paid-lv')"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="card m-1">
      <div class="card-header">Ресто</div>
      <div class="card-body">
        <div class="row">
          <div class="col mb-2">
            <div class="row g-3 align-items-center">
              <div class="col-auto">
                <label for="change-euro" class="form-label mr-1">Евро</label>
              </div>
              <div class="col-auto">
                <input
                  id="change-euro"
                  type="text"
                  class="form-control-sm"
                  :value="changeEuro"
                  disabled
                />
              </div>
            </div>
          </div>
          <div class="col">
            <div class="row g-3 align-items-center">
              <div class="col-auto">
                <label for="change-lv" class="form-label mr-1">Лева</label>
              </div>
              <div class="col-auto">
                <input
                  id="change-lv"
                  type="text"
                  class="form-control-sm"
                  :value="changeLv"
                  disabled
                />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div class="row text-center mt-2">
      <div class="col">
        <button type="button" class="btn btn-primary" @click="clear">Изчисти</button>
      </div>
    </div>
    <!-- /.row -->
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      eurBgn: 1.95583,
      endPriceLv: '',
      endPriceEuro: '',
      amountPaidLv: '',
      amountPaidEuro: '',
      focusedElement: '',
    }
  },
  computed: {
    changeLv() {
      if (this.endPriceLv && this.amountPaidLv) {
        return (this.amountPaidLv - this.endPriceLv).toFixed(2)
      }

      return ''
    },
    changeEuro() {
      if (this.endPriceEuro && this.amountPaidEuro) {
        return (this.amountPaidEuro - this.endPriceEuro).toFixed(2)
      }

      return ''
    },
  },
  methods: {
    convertToEur(value, linkedField) {
      switch (linkedField) {
        case 'endPriceEuro':
          this.endPriceEuro = this.calcLvToEur(value)
          break
        case 'amountPaidEuro':
          this.amountPaidEuro = this.calcLvToEur(value)
          break
        case 'changeEuro':
          this.changeEuro = this.calcLvToEur(value)
          break
      }
    },
    convertToBgn(value, linkedField) {
      switch (linkedField) {
        case 'endPriceLv':
          this.endPriceLv = this.calcEurToLv(value)
          break
        case 'amountPaidLv':
          this.amountPaidLv = this.calcEurToLv(value)
          break
        case 'changeLv':
          this.changeLv = this.calcEurToLv(value)
          break
      }
    },
    calcLvToEur(value) {
      if (Number.isNaN(Number.parseFloat(value))) {
        return ''
      }
      return (value / this.eurBgn).toFixed(2)
    },
    calcEurToLv(value) {
      if (Number.isNaN(Number.parseFloat(value))) {
        return ''
      }
      return (value * this.eurBgn).toFixed(2)
    },
    setFocus(element) {
      this.focusedElement = element
    },
    clear() {
      this.endPriceLv = '';
      this.endPriceEuro = '';
      this.amountPaidLv = '';
      this.amountPaidEuro = '';
    }
  },
  watch: {
    endPriceLv(newVal) {
      console.log(this.focusedElement)
      if (this.focusedElement === 'end-price-lv') {
        this.endPriceEuro = this.calcLvToEur(newVal)
      }
    },
    endPriceEuro(newVal) {
      console.log(this.focusedElement)
      if (this.focusedElement === 'end-price-euro') {
        this.endPriceLv = this.calcEurToLv(newVal)
      }
    },
    amountPaidLv(value) {
      console.log(this.focusedElement)
      if (this.focusedElement === 'amount-paid-lv') {
        this.amountPaidEuro = this.calcLvToEur(value)
      }
    },
    amountPaidEuro(value) {
      console.log(this.focusedElement)
      if (this.focusedElement === 'amount-paid-euro') {
        this.amountPaidLv = this.calcEurToLv(value)
      }
    },
  },
}
</script>

<style scoped></style>
