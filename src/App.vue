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
        >Multicamp Flutter Kampı <br />
        Sonuçları ⚡️</CHeading
      >
      <CBox mt="3" mb="3" align="center" style="">
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
          left-icon=""
          variant-color="blue"
          style="border-radius:20px"
          :isDisabled="!selected"
          mt="6"
          @click="open"
          >Sonuçlarımı getir</CButton
        >
      </CBox>
    </CBox>
    <template v-if="selected">
      <c-modal :is-open="isOpen" :on-close="close" style="z-index: 1;">
        <c-modal-content ref="content">
          <c-modal-header>Sonucunuz</c-modal-header>
          <c-modal-close-button />
          <c-modal-body>
            Notun: {{ getData.total }} <br /><br />
            Açıklama: {{ getData.description }}
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
  },
  data() {
    return {
      isOpen: false,
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
    close() {
      this.isOpen = false;
    },
  },
};
</script>