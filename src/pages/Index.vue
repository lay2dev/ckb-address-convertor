<template>
  <q-page class="">
    <div class="q-pa-lg">
      <div class="row">
        <div class="q-pa-md col-lg-5 col-xs-12">
          <b class="q-py-lg" style="font-size: 20px">ETH Address</b>
           <q-icon class="q-pa-md"
          name="content_copy"
          @click="copy(ethAddress)"
          style="font-size: 16px" />
          <q-input v-model="ethAddress" filled type="textarea" counter>
            <template v-slot:append>
              <q-icon
                name="close"
                @click="clear()"
                class="cursor-pointer"
              />
            </template>
            <template v-slot:hint> Field hint </template>
          </q-input>
        </div>
        <div class="q-pa-sm self-center">
          <q-btn round color="primary" icon="autorenew" style="font-size: 32px" @click="trans()" />
        </div>
        <div class="q-pa-md col-lg-5 col-xs-12">
          <b class="q-py-lg" style="font-size: 20px">CKB Address</b>
           <q-icon class="q-pa-md"
          name="content_copy"
          @click="copy(ckbAddress)"
          style="font-size: 16px" />
          <q-input v-model="ckbAddress" filled type="textarea" counter>
            <template v-slot:append>
              <q-icon
                name="close"
                @click="clear()"
                class="cursor-pointer"
              />
            </template>
            <template v-slot:hint> Field hint </template>
          </q-input>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import PWCore, {
  Address, AddressType, Web3ModalProvider,
  // EthSigner,
} from '@lay2/pw-core';

export default {
  name: 'PageIndex',
  data() {
    return {
      ethAddress: '',
      ckbAddress: '',
    };
  },

  async mounted() {
    this.pw = await new PWCore('https://mainnet.ckb.dev').init(
      new Web3ModalProvider(),
    );
  },

  methods: {
    trans() {
      if (this.ethAddress !== '') {
        this.Address = new Address(this.ethAddress, AddressType.eth);
        this.ckbAddress = this.Address.toCKBAddress();
      } else if (this.ckbAddress !== '') {
        this.Address = new Address(this.ckbAddress, AddressType.ckb);
        this.ethAddress = this.Address.toLockScript().args;
      }
    },
    copy(content) {
      const aux = document.createElement('input');
      aux.setAttribute('value', content);
      document.body.appendChild(aux);
      aux.select();
      document.execCommand('copy');
      document.body.removeChild(aux);
    },
    clear() {
      this.ethAddress = '';
      this.ckbAddress = '';
    },
  },

};
</script>
