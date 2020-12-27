<template>
  <div id="app">
    <div class="container my-10">
      <div class="row">
        <div class="col-xl-6 col-lg-8 col-md-10 mx-auto">
         <h1>Сколько весит конверт</h1>
          <p class="text-secondary">Если на конверт нанесена литера А (такие конверты обычно называют маркированные), то в его стоимость уже входит вес 20 гр.</p>
          <b-form-group label="Маркировка на конверте">
            <b-form-radio v-model="isMarked" aria-describedby="ariaDescribedby" name="marked" :value="true">Маркированный</b-form-radio>
            <b-form-radio v-model="isMarked" aria-describedby="ariaDescribedby" name="marked" :value="false">Не маркированный</b-form-radio>
          </b-form-group>
          <b-form-group label="Тип конверта">
            <b-form-select v-model="envelopWeight" :options="envelopsOptions" required @change="calc"/>
          </b-form-group>

          <b-form-group label="Количество листов А4">
            <b-form-input v-model="sheetsCount" type="number" placeholder="Количество листов" required @change="calc"/>
          </b-form-group>
<!--          <b-button variant="accent" @click="calc">Рассчитать</b-button>-->
          <div class="mt-10">
            <h6><span class="text-secondary">Вес конверта с листами:</span> {{totalWeight}} гр.</h6>
            <h6><span class="text-secondary">Номинал марок:</span> {{totalStamps ? totalStamps + ' коп.' : 'дополнительные марки не нужны'}}</h6>
            <div class="alert alert-info mt-8" v-if="!isMarked">Необходимо доклеить марка с литерой А</div>
          </div>
          <div class="mt-10 p-4 bg-light rounded">
            <b-link v-b-toggle="'collapse'" class="mb-4">Как производится рассчет?</b-link>
            <!-- Element to collapse -->
            <b-collapse id="collapse">
              <p>Все веса конвертов и напечатанных листов бумаги округлены в большую сторону до целого числа.</p>
              <ul>
                <li>Конверт E65 (евроконверт) весит 5 гр.</li>
                <li>Конверт С5 весит 7 гр.</li>
                <li>Конверт С4 весит 17 гр.</li>
              </ul>
              <p>Напечатанный на принтере лист бумаги А4 весит 5 гр.</p>
              <p>Марки практически невесомые, они исключены из рассчета.</p>
              <p>За каждые 20 гр. веса (кроме тех 20 гр. которые уже включены в стоимость конверта) необходимо доклеивать марки общим номиналом 4 коп.</p>
            </b-collapse>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      weight: 0,
      isMarked: true,
      envelopsOptions: [
        { value: 0, text: 'Выберите конверт', disabled: true },
        { value: 5, text: 'E65 – А4 втрое (5 гр.)' },
        { value: 7, text: 'C5 – А4 вдвое (7 гр.)' },
        { value: 17, text: 'C4 – А4 (17гр.)' },
      ],
      totalStamps: 0,
      sheetsCount: 0,
      sheetWeight: 5,
      envelopWeight: 0,
      totalWeight: 0,
      stepWeight: 20, //
      stepPrice: 4
    }
  },
  methods: {
    calc() {
      if(this.sheetsCount && this.envelopWeight) {
        this.totalWeight = this.sheetsCount * this.sheetWeight + this.envelopWeight;

        if(this.totalWeight > this.stepWeight) {
          const number = Math.ceil((this.totalWeight - 20) / this.stepWeight);
          console.log((this.totalWeight - 20) / this.stepWeight);

          this.totalStamps = number * this.stepPrice;
        }
      }
    }
  }
};
</script>
