<template>
  <Hero>
    <div class="inscricoes">
      <h1>Inscrições</h1>
      <svg
        v-if="mobileMode"
        id="arrows"
        height="50px"
        width="50px"
        viewBox="0 0 512 512"
        xmlns="http://www.w3.org/2000/svg"
      >
        <g fill="#5153ff">
          <path
            d="m320 424a24 24 0 0 1 -16.971-40.971l127.03-127.029-127.03-127.029a24 24 0 0 1 33.942-33.942l144 144a24 24 0 0 1 0 33.942l-144 144a23.928 23.928 0 0 1 -16.971 7.029z"
          />
          <path
            d="m184 424a24 24 0 0 1 -16.971-40.971l127.03-127.029-127.03-127.029a24 24 0 0 1 33.942-33.942l144 144a24 24 0 0 1 0 33.942l-144 144a23.928 23.928 0 0 1 -16.971 7.029z"
          />
          <path
            d="m48 424a24 24 0 0 1 -16.971-40.971l127.03-127.029-127.03-127.029a24 24 0 0 1 33.942-33.942l144 144a24 24 0 0 1 0 33.942l-144 144a23.928 23.928 0 0 1 -16.971 7.029z"
          />
        </g>
      </svg>
    </div>
  </Hero>
  <section class="signups">
    <div class="signupCards" v-if="!selectedInscricao">
      <inscricao-card
        nome="Estudante"
        preco="15"
        @click="selectedInscricao = 'Estudante'"
      />
      <inscricao-card
        nome="Estudante Universitário"
        preco="25"
        @click="selectedInscricao = 'Estudante Universitário'"
      />
      <inscricao-card
        nome="Geral"
        preco="40"
        @click="selectedInscricao = 'Geral'"
      />
    </div>
    <div class="forms" v-else>
      <div class="passe">
        <div class="voltar" @click="selectedInscricao = ''">
          <i class="fas fa-arrow-left"></i>
          <p>Voltar</p>
        </div>
        <h1>Passe {{ selectedInscricao }}</h1>
        <img src="@/assets/img.jpg" alt="" />
      </div>
      <form action="POST" @submit.prevent="submitForm">
        <label for="name">Nome completo *</label>
        <input type="text" id="name" v-model="fullName" required />
        <label for="email">Email *</label>
        <input type="email" id="email" v-model="email" required />
        <label for="foto">Foto</label>
        <label
          for="instituicao"
          v-if="
            selectedInscricao === 'Estudante' ||
              selectedInscricao === 'Estudante Universitário'
          "
          >Instituição *</label
        >
        <input
          type="text"
          id="instituicao"
          v-model="instituicao"
          required
          v-if="
            selectedInscricao === 'Estudante' ||
              selectedInscricao === 'Estudante Universitário'
          "
        />
        <label
          v-if="
            selectedInscricao === 'Estudante' ||
              selectedInscricao === 'Estudante Universitário'
          "
          >Comprovativo de matricula <span>(em pdf)</span> *</label
        >
        <dropzone
          @drop.prevent="drop"
          @change="selectedFile"
          v-if="
            selectedInscricao === 'Estudante' ||
              selectedInscricao === 'Estudante Universitário'
          "
        />
        <span
          v-if="
            selectedInscricao === 'Estudante' ||
              selectedInscricao === 'Estudante Universitário'
          "
          class="fileInfo"
          >Ficheiro: {{ dropzoneFile.name }}</span
        >
        <label for="nif">NIF</label>
        <input type="text" id="nif" v-model="nif" />
        <div class="checkboxes">
          <label for="checkbox"
            >Autorizo o tratamento dos meus dados para fins de adquirir o
            bilhete *</label
          >
          <input type="checkbox" id="checkbox" v-model="isChecked" required />
        </div>

        <p>
          Nota: Após confirmadas as credenciais enviaremos um email com os dados
          para o pagamento do respetivo bilhete
        </p>
        <blob-button mensagem="enviar" class="formButton" />
      </form>
    </div>
  </section>
</template>

<script>
import BlobButton from "@/components/BlobButton.vue";
import InscricaoCard from "@/components/InscricaoCard.vue";
import Dropzone from "@/components/Dropzone.vue";
import Hero from "@/components/Hero.vue";
import { ref } from "vue";

export default {
  name: "Inscricoes",
  data() {
    return {
      selectedInscricao: "",
      fullName: "",
      email: "",
      instituicao: "",
      nif: "",
      isChecked: false,
    };
  },
  setup() {
    let dropzoneFile = ref("");

    const drop = (event) => {
      dropzoneFile.value = event.dataTransfer.files[0]; //Get first file in files array
    };

    const selectedFile = () => {
      dropzoneFile.value = document.querySelector(".dropzoneFile").files[0];
    };

    return { dropzoneFile, drop, selectedFile };
  },
  components: {
    BlobButton,
    InscricaoCard,
    Dropzone,
    Hero,
  },
  methods: {
    submitForm() {
      console.log("formz");
    },
  },
};
</script>

<style lang="scss" scoped>
$textColor: #7179f4;
$primaryColor: #5a68e0;

.inscricoes {
  margin-bottom: 10vh;
  margin-left: 10vw;

  h1 {
    font-size: 50px;
    color: $textColor;
    font-weight: 800;
    text-transform: uppercase;
    margin-bottom: 0;
  }
}

.signups {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding: 100px 50px;

  .signupCards {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
  }

  .forms {
    display: flex;
    justify-content: center;

    .passe {
      margin-right: 50px;

      .voltar {
        display: flex;
        justify-content: center;
        align-items: center;
        margin-top: -70px;
        margin-bottom: 20px;
        padding: 5px;
        border-radius: 50px;
        width: 100px;
        box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
        cursor: pointer;

        i {
          margin-right: 10px;
          font-size: 20px;
        }
      }

      h1 {
        margin-top: 0;
      }

      img {
        width: 350px;
      }
    }
  }

  form {
    position: relative;
    display: flex;
    flex-direction: column;

    .fileInfo {
      margin-top: 10px;
      margin-bottom: 15px;
      padding-left: 5px;
      color: #0010ff;
      font-weight: 300;
      font-style: italic;
    }

    label {
      color: #0010ff;
      font-weight: 300;
      font-style: italic;
      padding: 5px;
    }
    input,
    textarea {
      width: 25vw;
      padding: 10px;
      background-color: #9ca2fd;
      color: #ffffff;
      margin-bottom: 15px;
      border: none;
      border-radius: 12px;
      outline: none;
      resize: none;

      &::placeholder {
        /* Chrome, Firefox, Opera, Safari 10.1+ */
        color: #ffffff;
        opacity: 0.7; /* Firefox */
        font-size: 20px;
        font-style: italic;
        font-weight: 300;
      }

      &:-ms-input-placeholder {
        /* Internet Explorer 10-11 */
        color: #ffffff;
        font-size: 20px;
        font-style: italic;
        font-weight: 300;
      }

      &::-ms-input-placeholder {
        /* Microsoft Edge */
        color: re#FFFFFF;
        font-size: 20px;
        font-style: italic;
        font-weight: 300;
      }
    }

    .checkboxes {
      display: flex;
      flex-direction: row-reverse;
      align-items: center;
      justify-content: flex-end;

      input {
        margin-bottom: 3px;
        width: auto;
      }
    }

    .formButton {
      position: absolute;
      left: -50px;
      bottom: -225px;
    }
  }
}
</style>
