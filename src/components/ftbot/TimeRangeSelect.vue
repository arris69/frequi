<template>
  <b-card class="row">
    <b-list-group class="col-mb-4" horizontal="md">
      <b-form-group label="Start date" label-for="dp_dateFrom">
        <b-input-group>
          <b-input-group-prepend>
            <b-form-datepicker v-model="dateFrom" class="mb-2" button-only></b-form-datepicker>
          </b-input-group-prepend>
          <b-form-input
            id="dp_dateFrom"
            v-model="dateFrom"
            type="text"
            placeholder="YYYY-MM-DD"
            autocomplete="off"
          ></b-form-input>
        </b-input-group>
      </b-form-group>
      <b-form-group class="ml-2" label="End date" label-for="dp_dateTo">
        <b-input-group>
          <b-input-group-prepend>
            <b-form-datepicker v-model="dateTo" class="mb-2" button-only></b-form-datepicker>
          </b-input-group-prepend>
          <b-form-input
            id="dp_dateTo"
            v-model="dateTo"
            type="text"
            placeholder="YYYY-MM-DD"
            autocomplete="off"
          ></b-form-input>
        </b-input-group>
      </b-form-group>
      <label
        >Timerange: <b>{{ timeRange }}</b></label
      >
    </b-list-group>
  </b-card>
</template>

<script lang="ts">
import { Component, Vue, Emit, Prop } from 'vue-property-decorator';
import { dateFromString, dateStringToTimeRange, timestampToDateString } from '@/shared/formatters';

const now = new Date();
@Component({})
export default class TimeRangeSelect extends Vue {
  dateFrom = '';

  dateTo = '';

  @Prop() value!: string;

  @Emit('input')
  emitTimeRange() {
    return this.timeRange;
  }

  created() {
    if (!this.value) {
      this.dateFrom = timestampToDateString(new Date(now.getFullYear(), now.getMonth() - 1, 1));
    } else {
      const tr = this.value.split('-');
      if (tr[0]) {
        this.dateFrom = timestampToDateString(dateFromString(tr[0], 'YYYYMMDD'));
      }
      if (tr.length > 1 && tr[1]) {
        this.dateTo = timestampToDateString(dateFromString(tr[1], 'YYYYMMDD'));
      }
    }
    this.emitTimeRange();
  }

  updated() {
    this.emitTimeRange();
  }

  get timeRange() {
    if (this.dateFrom !== '' || this.dateTo !== '') {
      return `${dateStringToTimeRange(this.dateFrom)}-${dateStringToTimeRange(this.dateTo)}`;
    }
    return '';
  }
}
</script>

<style scoped>
.card-body {
  padding-bottom: 0.5rem;
  padding-top: 0.8rem;
  padding-left: 0.5rem;
  padding-right: 0.5rem;
}
</style>
