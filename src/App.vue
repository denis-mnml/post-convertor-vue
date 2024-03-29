<template>
  <div id="app" class="min-vh-100">
    <div class="container d-flex flex-column pt-10 pb-6 min-vh-100">
      <div class="row">
        <div class="col-xl-6 col-lg-8 col-md-10 mx-auto">
          <h1 class="h2">Сколько весит конверт</h1>
          <p class="text-secondary small">Если на конверт нанесена литера А (такие конверты обычно называют
            маркированные), то
            в его стоимость уже входит вес 20 гр.</p>
          <b-form-group label="Маркированный конверт?" class="mb-6">
            <b-form-radio v-model="isMarked" name="marked" :value="true">Маркированный</b-form-radio>
            <b-form-radio v-model="isMarked" name="marked" :value="false">Не маркированный</b-form-radio>
          </b-form-group>
          <b-form-group label="Тип конверта" class="mb-6">
            <div class="d-flex flex-wrap">
              <label class="envelope-radio h-100" v-for="item in envelopsOptions" :key="item.value">
                <input type="radio" name="envelope" v-model="envelopWeight" class="envelope-radio__input"
                       :value="item.value"
                       :checked="item.value === envelopWeight" @change="calc">
                <span class="envelope-radio__label">
                  <img :src="item.src" class="envelope-radio__img" :alt="'Конверт ' + item.text">
                  <p class="font-weight-bolder m-0">{{item.text}}</p>
                  <span class="envelope-radio__help" v-b-tooltip.hover :title="item.desc">?</span>
                </span>
              </label>
            </div>
          </b-form-group>
          <b-form-group label="Количество листов А4">
            <b-form-input v-model="sheetsCount" type="number" placeholder="Количество листов" required @change="calc"
                          @focus="$event.target.select()"/>
          </b-form-group>
          <div class="mt-10">
            <div class="alert alert-info mb-4" v-if="!isMarked">Необходимо доклеить марку с литерой А</div>
            <p class="font-weight-medium m-0">
              <span class="text-secondary">Вес конверта с листами:</span> <span class="h5 font-weight-bold">{{totalWeight}} гр.</span>
            </p>
            <p class="font-weight-medium">
              <span class="text-secondary">Номинал марок:</span> <span class="h5 font-weight-bold">{{totalStamps ? totalStamps + ' коп.' :
              'дополнительные марки не нужны'}}</span>
            </p>
          </div>
          <div class="mt-10 p-4 bg-accent-50 rounded">
            <b-link v-b-toggle="'collapse'" class="mb-4">Как производится рассчет?</b-link>
            <!-- Element to collapse -->
            <b-collapse id="collapse" class="pt-4">
              <p>Вес конвертов и напечатанных листов бумаги округлены в большую сторону до целого числа.</p>
              <ul class="pl-8">
                <li><span class="font-weight-bold">Конверт формата E65 (евроконверт)</span> вмещает стандартный лист
                  бумаги А4, сложенный втрое и весит 5 гр.
                </li>
                <li><span class="font-weight-bold">Конверт формата С5</span> вмещает лист бумаги А4, сложенный пополам и
                  весит 7 гр.
                </li>
                <li><span class="font-weight-bold">Конверт формата С4</span> вмещает лист бумаги А4 весит 17 гр.</li>
              </ul>
              <p>Напечатанный на принтере лист бумаги А4 весит 5 гр.</p>
              <p>Марки практически невесомые, они исключены из рассчета.</p>
              <p>За каждые 20 гр. веса (кроме тех 20 гр. которые уже включены в стоимость конверта) необходимо
                доклеивать марки общим номиналом 5 коп.</p>
            </b-collapse>
          </div>
        </div>
      </div>
      <div class="mt-10 text-center">
        <a href="http://mnml.by" target="_blank" class="small">by Denis Minimal</a>
      </div>
    </div>
  </div>
</template>

<script>

  export default {
    name: 'App',
    components: {},
    data() {
      return {
        weight: 0,
        isMarked: true,
        envelopsOptions: [
          { value: 5, text: 'E65', desc: 'Размер: А4 сложен втрое', src: 'img/e65.svg' },
          { value: 7, text: 'C5', desc: 'Размер: А4 сложен вдвое', src: 'img/c5.svg' },
          { value: 17, text: 'C4', desc: 'Размер: А4 целиком', src: 'img/c4.svg' },
        ],
        totalStamps: 0,
        sheetsCount: 0,
        sheetWeight: 5,
        envelopWeight: 5,
        totalWeight: 0,
        stepWeight: 20, //
        stepPrice: 5 // цена за каждые 20 грамм, коп.
      };
    },
    mounted() {
      this.calc();
    },
    methods: {
      calc() {
        this.totalWeight = this.sheetsCount * this.sheetWeight + this.envelopWeight;

        if(this.totalWeight > this.stepWeight) {
          const steps = Math.ceil((this.totalWeight - 20) / this.stepWeight);

          this.totalStamps = steps * this.stepPrice;
        } else {
          this.totalStamps = 0;
        }
      }
    }
  };
</script>
