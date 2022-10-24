<template>
  <v-row>
    <v-container color="black" height="100%" class="pa-5 card-principal">
      <div
        class="d-flex justify-md-space-between flex-wrap justify-center align-center"
      >
        <p class="white--text font-weight-bold headline mb-0">
          www.laradio.com.co
        </p>
        <div class="d-flex justify-md-space-between justify-center flex-wrap">
          <v-btn
            icon
            href="https://wa.link/dr1r57"
            target="_blank"
            class="ma-2"
          >
            <v-icon large color="white">mdi-whatsapp</v-icon>
          </v-btn>
          <v-btn
            icon
            href="https://www.facebook.com/iipuc.calatrava"
            target="_blank"
            class="ma-2"
          >
            <v-icon large color="white">mdi-facebook</v-icon>
          </v-btn>
          <v-btn
            icon
            href="https://instagram.com/ipuccalatrava?igshid=YmMyMTA2M2Y="
            target="_blank"
            class="ma-2"
          >
            <v-icon large color="white">mdi-instagram</v-icon>
          </v-btn>
          <v-btn
            icon
            href="https://www.youtube.com/channel/UCOs-eUekXs6Y9u5GGTuVTEA"
            target="_blank"
            class="ma-2"
          >
            <v-icon large color="white">mdi-youtube</v-icon>
          </v-btn>
        </div>
      </div>
      <v-img src="/logo.png" max-width="300" class="mx-auto mt-10 d-xl-none" />
      <v-img
        src="/logo.png"
        max-width="600"
        class="mx-auto mt-10 d-none d-xl-flex"
      />
      <p class="white--text text-center title mt-10">Programación Radial:</p>
      <div class="d-flex justify-space-around flex-wrap">
        <v-dialog
          v-for="item in programacion"
          :key="item.id"
          v-model="removeItemDialogs[item.id]"
          :retain-focus="false"
          width="600px"
        >
          <template #activator="{ on, attrs }">
            <v-card
              min-width="190px"
              v-bind="attrs"
              class="pa-3 px-10 ma-2 white--text text-center headline font-weight-medium w-100 w-md-none"
              :class="item.class"
              v-on="on"
            >
              <p class="mb-0">{{ item.nombredelBoton }}</p>
            </v-card>
          </template>
          <v-img :src="item.image" />
        </v-dialog>
      </div>
      <p class="white--text mt-10 text-center">
        La Radio es una emisora Cristiana del Nombre de Jesús que predica un
        señor, una fe y un bautismo. Transmitiendo desde el barrido Calatrava en
        el municipio de Itagüí - Colombia. Te invitamos a ser parte de ésta
        misión.
      </p>
      <hr width="100%" class="hr-styles mt-10" />
      <v-card
        color="#002978"
        class="card-principal"
        flat
        tile
        min-height="200"
        width="100%"
      >
        <v-card
          color="#002978"
          min-height="190"
          flat
          tile
          class="d-flex justify-space-around align-center flex-wrap pa-5"
        >
          <div>
            <p class="display-1 font-weight-bold white--text mb-0">
              LaRadio.com.co
            </p>
            <v-card color="red" class="d-flex justify-center" flat>
              <p class="white--text mb-0 font-weight-bold">
                Transmisión en Vivo
                <v-icon small color="white" class="ml-5 parpadea"
                  >mdi-checkbox-blank-circle</v-icon
                >
              </p>
            </v-card>
          </div>
          <v-card class="d-flex px-2" color="#002978" flat>
            <audio-player
              class="mx-0"
              url="http://148.72.152.177:8390/stream"
              playerid="audio-player"
            >
            </audio-player>
            <!-- <div class="cc_player" data-username="laradio2">Cargando ...</div> -->
          </v-card>
        </v-card>
      </v-card>
      <v-card tile flat class="pa-10">
        <v-row align="center">
          <v-col cols="12" md="3" sm="12">
            <p class="text-center text-md-left">
              <span class="font-weight-bold"
                >¿Quieres comunicarte con nosotros?</span
              ><br />
              Déjanos un mensaje <br />
              <span class="font-weight-bold">Contacto:</span> + 57 311 387 0322
            </p>
          </v-col>
          <v-col cols="12" md="6" sm="12">
            <div class="mx-5">
              <v-form id="form" ref="form" v-model="valid">
                <v-text-field
                  id="from_name"
                  v-model="from_name"
                  name="from_name"
                  width="100%"
                  label="Nombre"
                  placeholder="Nombre"
                  outlined
                  dense
                  class="mb-0"
                  :rules="[validationRules.required, validationRules.min]"
                ></v-text-field>
                <v-text-field
                  id="from_email"
                  v-model="from_email"
                  name="from_email"
                  width="100%"
                  label="Correo"
                  placeholder="Correo"
                  outlined
                  dense
                  class="mb-0"
                  :rules="[validationRules.required, validationRules.email]"
                ></v-text-field>
                <v-text-field
                  id="from_phone_number"
                  v-model="from_phone_number"
                  name="from_phone_number"
                  width="100%"
                  label="Telefono"
                  placeholder="Telefono"
                  outlined
                  dense
                  class="mb-0"
                  :rules="[
                    validationRules.required,
                    validationRules.number,
                    validationRules.max,
                    validationRules.minPhone,
                  ]"
                ></v-text-field>
                <v-textarea
                  id="message"
                  v-model="message"
                  name="message"
                  width="100%"
                  outlined
                  dense
                  label="Mensaje"
                  class="mb-0"
                  :rules="[validationRules.required, validationRules.min]"
                ></v-textarea>
                <v-btn
                  :disabled="loading"
                  :loading="loading"
                  block
                  color="#002978"
                  elevation="0"
                  class="white--text"
                  @click="send()"
                  >Enviar</v-btn
                >
              </v-form>
            </div>
          </v-col>
          <v-col cols="12" md="3" sm="12">
            <p class="text-center mb-1">Descarga la App:</p>
            <a
              href="https://play.google.com/store/apps/details?id=la.radio14"
              target="_blank"
            >
              <v-img
                src="/play.png"
                max-width="200"
                max-height="60"
                class="mx-auto"
              />
            </a>
          </v-col>
          <v-snackbar v-model="snackbar" :color="color" timeout="1500" text>
            {{ text }}
          </v-snackbar>
        </v-row>
      </v-card>
    </v-container>
    <video src="~/assets/video.mp4" autoplay muted loop class="video" />
  </v-row>
</template>

<script>
import isEmail from "validator/lib/isEmail";
import isNumeric from "validator/lib/isNumeric";
import emailjs from "emailjs-com";
export default {
  name: "IndexPage",
  data: () => ({
    removeItemDialogs: {},
    programacion: [
      {
        id: 1,
        nombredelBoton: "Lunes",
        image: "/lunes.jpg",
        class: "lunes",
      },
      {
        id: 2,
        nombredelBoton: "Martes",
        image: "/martes.jpg",
        class: "martes",
      },
      {
        id: 3,
        nombredelBoton: "Miercoles",
        image: "/miercoles.jpg",
        class: "miercoles",
      },
      {
        id: 4,
        nombredelBoton: "Jueves",
        image: "/jueves.jpg",
        class: "jueves",
      },
      {
        id: 5,
        nombredelBoton: "Viernes",
        image: "/viernes.jpg",
        class: "viernes",
      },
      {
        id: 6,
        nombredelBoton: "Sábado",
        image: "/sabado.jpg",
        class: "sabado",
      },
      {
        id: 7,
        nombredelBoton: "Domingo",
        image: "/domingo.jpg",
        class: "domingo",
      },
    ],
    message: "",
    from_name: "",
    from_email: "",
    from_phone_number: "",
    loading: false,
    text: "",
    color: "",
    snackbar: false,
    valid: false,
    validationRules: {
      required: (value) => !!value || "Campo requerido",
      min: (v) => v?.length >= 5 || "Minimo 5 caracteres",
      minPhone: (v) => v?.length >= 10 || "Minimo 10 caracteres",
      max: (v) => v?.length <= 10 || "Maximo 10 caracteres",
      email: (v) => {
        if (v?.length > 0) {
          return !!isEmail(v) || "Correo invalido";
        }
        return true;
      },
      number: (v) => !!isNumeric(v) || "Solo numeros",
    },
  }),
  methods: {
    send() {
      this.valid = this.$refs.form.validate();
      if (this.valid) {
        this.loading = true;
        emailjs
          .sendForm(
            "service_yonnzwx",
            "template_z6spiu8",
            "#form",
            "rlSPL-z0MIvyynAzQ"
          )
          .then(
            (result) => {
              this.snackbar = true;
              this.text = "Mensaje enviado.";
              this.color = "primary";
              this.loading = false;
              this.message = "";
              this.from_name = "";
              this.from_phone_number = "";
              this.from_email = "";
              this.$refs.form.resetValidation();
            },
            (error) => {
              console.log(error.text);
              this.snackbar = true;
              this.text = "Mensaje no enviado, inténtalo más tarde.";
              this.color = "red";
              this.loading = false;
              this.message = "";
              this.from_name = "";
              this.from_phone_number = "";
              this.from_email = "";
            }
          )
          .catch(() => {
            this.snackbar = true;
            this.text = "Mensaje no enviado, inténtalo más tarde.";
            this.color = "red";
            this.loading = false;
            this.message = "";
            this.from_name = "";
          });
      } else {
        this.snackbar = true;
        this.color = "red";
        this.text = "Mensaje no enviado, inténtalo más tarde.";
      }
    },
  },
};
</script>
<style>
.card-principal {
  z-index: 100 !important;
}

.video {
  object-fit: cover;
  height: 100%;
  width: 100%;
  position: absolute;
  z-index: 0;
  top: 0;
  left: 0;
}

.lunes {
  background: rgb(138, 28, 169);
  background: linear-gradient(
    0deg,
    rgba(138, 28, 169, 1) 0%,
    rgba(185, 7, 163, 1) 50%
  );
}

.martes {
  background: rgb(198, 72, 76);
  background: linear-gradient(
    0deg,
    rgba(198, 72, 76, 1) 0%,
    rgb(211, 46, 51) 50%
  );
}

.miercoles {
  background: rgb(40, 25, 121);
  background: linear-gradient(
    0deg,
    rgba(40, 25, 121, 1) 0%,
    rgba(71, 36, 150, 1) 50%
  );
}

.jueves {
  background: rgb(138, 28, 169);
  background: linear-gradient(
    0deg,
    rgba(138, 28, 169, 1) 0%,
    rgba(185, 7, 163, 1) 50%
  );
}

.viernes {
  background: rgb(198, 72, 76);
  background: linear-gradient(
    0deg,
    rgba(198, 72, 76, 1) 0%,
    rgb(211, 46, 51) 50%
  );
}

.sabado {
  background: rgb(40, 25, 121);
  background: linear-gradient(
    0deg,
    rgba(40, 25, 121, 1) 0%,
    rgba(71, 36, 150, 1) 50%
  );
}

.domingo {
  background: rgb(138, 28, 169);
  background: linear-gradient(0deg, rgb(10, 135, 125) 0%, rgb(8, 173, 118) 50%);
}

.hr-styles {
  border: 5px solid white;
}

.parpadea {
  animation-name: parpadeo;
  animation-duration: 2s;
  animation-timing-function: linear;
  animation-iteration-count: infinite;

  -webkit-animation-name: parpadeo;
  -webkit-animation-duration: 2s;
  -webkit-animation-timing-function: linear;
  -webkit-animation-iteration-count: infinite;
}

@-moz-keyframes parpadeo {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@-webkit-keyframes parpadeo {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}

@keyframes parpadeo {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
