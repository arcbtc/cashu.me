<template>
  <q-table
    dense
    flat
    :rows="historyTokens"
    :columns="historyTable.columns"
    no-data-label="There are no tokens here yet"
    :filter="historyTable.filter"
    :pagination="historyTable.pagination"
  >
    <template v-slot:body="props">
      <q-tr :props="props">
        <q-td key="status" :props="props">
          <div v-if="props.row.status == 'pending'">
            <q-icon
              @click="showTokenDialog(props.row.token)"
              name="settings_ethernet"
              color="grey"
            >
              <q-tooltip>Pending</q-tooltip>
            </q-icon>
            <q-icon
              name="sync"
              size="xs"
              color="grey"
              class="q-mr-xs cursor-pointer"
              @click="checkTokenSpendable(props.row.token)"
            >
              <q-tooltip>Check status</q-tooltip>
            </q-icon>
          </div>
          <div v-if="props.row.status === 'paid'">
            <q-icon
              v-if="props.row.amount > 0"
              name="call_received"
              color="green"
              ><q-tooltip>Received</q-tooltip></q-icon
            >
            <q-icon v-if="props.row.amount < 0" name="call_made" color="red"
              ><q-tooltip>Paid</q-tooltip></q-icon
            >
            <!-- <q-icon name="props.row.amount < 0 ? 'call_made' : 'call_received'" color="green"></q-icon> -->
          </div>
        </q-td>
        <q-td
          key="amount"
          :props="props"
          :class="props.row.amount > 0 ? 'text-green-13 text-weight-bold' : ''"
        >
          <div>{{ formatSat(props.row.amount) }}</div>
        </q-td>
        <q-td key="date" :props="props">
          <div>{{ props.row.date }}</div>
        </q-td>
        <!-- <q-td key="memo" :props="props">
                            <div>{{props.row.memo}}</div>
                        </q-td> -->
        <q-td key="token" :props="props">
          <div @click="copyText(props.row.token)">
            {{ shortenString(props.row.token, 10, 40) }}
            <q-tooltip>Click to copy</q-tooltip>
          </div>
        </q-td>
      </q-tr>
    </template>
  </q-table>
</template>
<script>
import { defineComponent } from "vue";
import { shortenString } from "src/js/string-utils";

export default defineComponent({
  name: "HistoryTable",
  mixins: [windowMixin],
  props: {
    proofs: Array,
    activeProofs: Array,
    mints: Array,
    tickerShort: String,
    activeMintUrl: String,
    historyTokens: Array,
    showTokenDialog: Function,
    checkTokenSpendable: Function,
  },
  data: function () {
    return {
      historyTable: {
        columns: [
          {
            name: "status",
            align: "left",
            label: "",
            field: "status",
            sortable: true,
          },
          {
            name: "amount",
            align: "left",
            label: "Amount",
            field: "amount",
            sortable: true,
          },
          {
            name: "date",
            align: "left",
            label: "Date",
            field: "date",
            sortable: true,
          },
          // {
          //   name: 'memo',
          //   align: 'left',
          //   label: 'Memo',
          //   field: 'memo',
          //   sortable: true
          // },
          {
            name: "token",
            align: "left",
            label: "Token",
            field: "token",
            sortable: false,
          },
        ],
        pagination: {
          sortBy: "date",
          descending: true,
          rowsPerPage: 5,
        },
        filter: null,
      },
    };
  },
  methods: {
    shortenString: function (s) {
      return shortenString(s, 20, 10);
    },
  },
  created: function () {},
});
</script>
