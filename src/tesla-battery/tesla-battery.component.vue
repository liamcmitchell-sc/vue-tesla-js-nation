<template>
  <form class="tesla-battery">
    <h1>{{ title }}</h1>

    <tesla-car :wheels="wheels.value" :speed="speed.value" />

    <tesla-stats :stats="stats" />

    <div class="tesla-controls cf">
      <tesla-counter title="Speed" :value="speed" />

      <div class="tesla-climate cf">
        <tesla-counter title="Outside Temperature" :value="temperature" />
        <tesla-climate :climate="climate" :limitHeat="limitHeat" />
      </div>

      <tesla-wheels :wheels="wheels" />
    </div>
    <div class="tesla-battery__notice">
      <p>
        The actual amount of range that you experience will vary based on your
        particular use conditions. See how particular use conditions may affect
        your range in our simulation model.
      </p>
      <p>
        Vehicle range may vary depending on the vehicle configuration, battery
        age and condition, driving style and operating, environmental and
        climate conditions.
      </p>
    </div>
  </form>
</template>

<script>
import TeslaCar from './components/tesla-car.component';
import TeslaStats from './components/tesla-stats.component';
import TeslaCounter from './components/tesla-counter.component';
import TeslaClimate from './components/tesla-climate.component';
import TeslaWheels from './components/tesla-wheels.component';

import teslaService from './tesla-battery.service';

export default {
  name: 'tesla-battery',
  components: {
    TeslaCar,
    TeslaStats,
    TeslaCounter,
    TeslaClimate,
    TeslaWheels,
  },
  created() {
    this.metrics = teslaService.getModelData();
  },
  data() {
    return {
      title: 'Ranger Per Charge',
      models: ['60', '60D', '75', '75D', '90D', 'P100D'],
      wheels: {
        sizes: [19, 21],
        value: 19,
        focused: null,
      },
      climate: {
        value: true,
        focused: false,
      },
      temperature: {
        value: 20,
        focused: false,
        min: -10,
        max: 40,
        step: 10,
      },
      speed: {
        value: 55,
        focused: false,
        min: 45,
        max: 70,
        step: 5,
      },
      metrics: [],
    };
  },
  computed: {
    stats() {
      return this.models.map(model => {
        const miles = this.metrics[model][this.wheels.value][
          this.climate.value ? 'on' : 'off'
        ].speed[this.speed.value][this.temperature.value];
        return {
          model,
          miles,
        };
      });
    },
    limitHeat() {
      return this.temperature.value > 10;
    },
  },
};
</script>
