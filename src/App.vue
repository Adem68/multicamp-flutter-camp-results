<template>
  <div class="container">
    <CBox
      v-bind="mainStyles[colorMode]"
      d="flex"
      w="100vw"
      h="100vh"
      flex-dir="column"
      justify-content="center"
    >
      <CBox mb="3" align="right">
        <CIconButton
          mr="3"
          :icon="colorMode === 'light' ? 'moon' : 'sun'"
          @click="toggleColorMode"
          :aria-label="
            `Switch to ${colorMode === 'light' ? 'dark' : 'light'} mode`
          "
        />
      </CBox>
      <CHeading textAlign="center" mb="4"
        >Multicamp Flutter<br />
        Kampı Sonuçları</CHeading
      >
      <CBox mt="3" mb="3" align="center">
        <c-select
          v-model="selected"
          placeholder="İsminizi seçiniz"
          width="50%"
          max-width="300px"
          style="z-index: 0;"
          :background="mainStyles[colorMode].dialogColor"
        >
          <template v-for="result in results">
            <option :key="result.id" :value="result.id">
              {{ result.name }}
            </option>
          </template>
        </c-select>
        <CButton
          variant-color="blue"
          style="border-radius:20px"
          :isDisabled="!selected"
          mt="6"
          @click="open"
          >Sonuçlarımı getir</CButton
        >
      </CBox>
      <CBox mt="3" mb="3" align="center"
        ><CButton
          left-icon="info"
          mt="3"
          variant-color="blue"
          style="border-radius:20px"
          @click="openImg"
          variant="outline"
          >Puan Tablosu</CButton
        >
      </CBox>
    </CBox>

    <template v-if="selected">
      <c-modal :is-open="isOpen" :on-close="close" style="z-index: 1;">
        <c-modal-content ref="content">
          <c-modal-header>Sonucun</c-modal-header>
          <c-modal-close-button />
          <c-modal-body>
            Notun: <b>{{ getData.total }}</b>
            <div style="margin-top:30px">
              <b>Puanların:</b>
            </div>
            <div style="margin-top:15px">
              <span> Mimari: {{ getData.architecture }},</span>
              <span> Splash: {{ getData.splash }},</span>
              <span> Login: {{ getData.auth }},</span>
              <span> Haberler: {{ getData.news }},</span>
              <span> Arama: {{ getData.search }},</span>
              <span> Haber Detay: {{ getData.news_detail }},</span>
              <span> Geliştirici Sayfası: {{ getData.developer }},</span>
              <span> Lisanslar: {{ getData.licenses }}</span>
              <span v-if="getData.theme > 0">, Tema: {{ getData.theme }}</span>
              <span v-if="getData.lang > 0">, Dil: {{ getData.lang }}</span>
              <span v-if="getData.bonus > 0">, Bonus: {{ getData.bonus }}</span>
            </div>
            <div style="margin-top:30px">
              Açıklama: {{ getData.description }}
            </div>
          </c-modal-body>
          <c-modal-footer>
            <c-button variant-color="blue" mr="3" @click="close">
              Tamam
            </c-button>
          </c-modal-footer>
        </c-modal-content>
        <c-modal-overlay />
      </c-modal>
    </template>
    <template v-if="isOpenImg">
      <c-modal :is-open="isOpenImg" :on-close="close" size="6xl">
        <c-modal-content ref="content">
          <c-modal-header>Puan Tablosu</c-modal-header>
          <c-modal-close-button />
          <c-modal-body>
            <c-box :display="{ md: 'flex', sm: 'none', xs: 'none' }">
              <c-image
                :src="require('@/assets/puan_tablosu.png')"
                alt="Puan Tablosu"
              />
            </c-box>
            <c-box :display="{ md: 'none', sm: 'flex' }">
              <c-image
                :src="require('@/assets/puan_tablosu_mobil.png')"
                alt="Puan Tablosu"
              />
            </c-box>
          </c-modal-body>
          <c-modal-footer>
            <c-button variant-color="blue" mr="3" @click="closeImg">
              Tamam
            </c-button>
          </c-modal-footer>
        </c-modal-content>
        <c-modal-overlay />
      </c-modal>
    </template>
  </div>
</template>

<script>
import {
  CBox,
  CButton,
  CIconButton,
  CHeading,
  CSelect,
  CModal,
  CModalOverlay,
  CModalContent,
  CModalHeader,
  CModalFooter,
  CModalBody,
  CModalCloseButton,
  CImage,
} from "@chakra-ui/vue";
import result_json from "@/assets/results.json";

export default {
  name: "App",
  inject: ["$chakraColorMode", "$toggleColorMode"],
  components: {
    CBox,
    CButton,
    CIconButton,
    CHeading,
    CSelect,
    CModal,
    CModalOverlay,
    CModalContent,
    CModalHeader,
    CModalFooter,
    CModalBody,
    CModalCloseButton,
    CImage,
  },
  data() {
    return {
      isOpen: false,
      isOpenImg: false,
      results: result_json,
      selected: null,
      mainStyles: {
        dark: {
          bg: "gray.700",
          color: "whiteAlpha.900",
          dialogColor: "#394353",
        },
        light: {
          bg: "white",
          color: "gray.900",
          dialogColor: "#FFFFFF",
        },
      },
      imgUrl: "../assets/puan_tablosu.png",
    };
  },
  computed: {
    colorMode() {
      return this.$chakraColorMode();
    },
    theme() {
      return this.$chakraTheme();
    },
    toggleColorMode() {
      return this.$toggleColorMode;
    },
    getData() {
      let result = this.results.find((r) => r.id === parseInt(this.selected));
      console.log(result);
      return result;
    },
  },
  methods: {
    open() {
      this.isOpen = true;
    },
    openImg() {
      this.isOpenImg = true;
    },
    close() {
      this.isOpen = false;
    },
    closeImg() {
      this.isOpenImg = false;
    },
  },
};
</script>