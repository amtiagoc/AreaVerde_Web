<template>
  <v-container id="login">
    <navbar :color="color" :flat="flat" />
    <v-row align="center" justify="center">
      <v-col cols="12" sm="10">
        <v-card class="elevation-6 mt-10">
          <componenteDialog
            v-if="this.dialogError == true"
            :estadoDialog="true"
            :tituloMensaje="'Error'"
            :mensaje="'Verifique que los datos sean los correctos'"
          />
          <v-window v-model="step">
            <v-window-item :value="1">
              <v-row>
                <v-col cols="12" sm="6">
                  <v-card-text class="mt-12">
                    <h2 class="text-center">Login to your account</h2>
                    <h4 class="text-center grey--text">
                      To access your account please fill in the <br />
                      information below.
                    </h4>
                    <br />
                    <v-row align="center" justify="center">
                      <v-col cols="12" sm="8">
                        <v-form
                          ref="formLogin"
                          v-model="valid"
                          class="pa-3 pt-4"
                          lazy-validation
                          color="#ccf2f4"
                        >
                          <v-text-field
                            label="usuario"
                            v-model="usuario.usuario"
                            :rules="rules.required"
                            outlined
                            dense
                            color="green"
                            autocomplete="false"
                            class="mt-16"
                          >
                          </v-text-field>
                          <v-text-field
                            label="password"
                            v-model="usuario.password"
                            :rules="rules.required"
                            outlined
                            dense
                            color="green"
                            autocomplete="false"
                            type="password"
                          >
                          </v-text-field>

                          <v-btn
                            color="green"
                            @click="ingreso()"
                            dark
                            block
                            tile
                          >
                            Log in
                          </v-btn>
                        </v-form>
                        <h5 class="text-center grey--text mt-4 mb-3">
                          Or log in using a social media account:
                        </h5>
                      </v-col>
                    </v-row>
                  </v-card-text>
                </v-col>
                <v-col cols="12" sm="6" class="green rounded-bl-xl">
                  <div style="text-align: center; padding: 180px 0">
                    <v-card-text class="white--text">
                      <h2 class="text-center">Don't have an account yet?</h2>
                      <h4 class="text-center">
                        Let's get you all set up so you can start helping <br />
                        the world be a better and CLEANER place!
                      </h4>
                    </v-card-text>
                    <div class="text-center">
                      <v-btn tile outlined dark @click="step++">
                        Sign up
                      </v-btn>
                    </div>
                  </div>
                </v-col>
              </v-row>
            </v-window-item>
            <!-- ----------------------------------------------------------------------------------------------------------------------------------------  -->
            <v-window-item :value="2">
              <v-row>
                <v-col cols="12" sm="6" class="green rounded-br-xl">
                  <div style="text-align: center; padding: 180px 0">
                    <v-card-text class="white--text">
                      <h2 class="text-center">Ya tienes una cuenta?</h2>
                      <h4 class="text-center">
                        Ingresa a para poder realizar reportes y ver
                        <br />
                        informacion mas detallada
                      </h4>
                    </v-card-text>
                    <div class="text-center">
                      <v-btn tile outlined dark @click="step--"> Log in </v-btn>
                    </div>
                  </div>
                </v-col>

                <v-col cols="12" sm="6">
                  <v-card-text class="mt-12">
                    <h2 class="text-center">Crea una cuenta.</h2>
                    <h4 class="text-center grey--text">
                      Por favor ingresa los datos necesarios para poder crear
                      <br />
                      una nueva cuenta
                    </h4>
                    <v-row align="center" justify="center">
                      <v-col cols="12" sm="12">
                        <v-form
                          ref="formRegistro"
                          v-model="valid"
                          class="pa-3 pt-4"
                          lazy-validation
                          color="#ccf2f4"
                        >
                          <v-text-field
                            label="name"
                            outlined
                            v-model="usuario.nombre"
                            :rules="rules.texto"
                            dense
                            color="green"
                            autocomplete="false"
                            class="mt-4"
                          >
                          </v-text-field>

                          <v-text-field
                            label="Documento Identificación"
                            outlined
                            v-model="usuario.cedula"
                            :rules="rules.doc"
                            dense
                            color="green"
                            autocomplete="false"
                            class="mt-4"
                          >
                          </v-text-field>

                          <v-text-field
                            label="Celular"
                            outlined
                            v-model="usuario.celular"
                            dense
                            :rules="rules.celular"
                            color="green"
                            autocomplete="false"
                            class="mt-4"
                          >
                          </v-text-field>

                          <v-text-field
                            label="Correo"
                            outlined
                            v-model="usuario.correo"
                            dense
                            :rules="rules.emailRules"
                            color="green"
                            autocomplete="false"
                            class="mt-4"
                            type="email"
                          >
                          </v-text-field>

                          <v-text-field
                            label="Usuario"
                            outlined
                            dense
                            v-model="usuario.usuario"
                            :rules="rules.required"
                            color="green"
                            autocomplete="false"
                            class="mt-4"
                          >
                          </v-text-field>

                          <v-text-field
                            label="Contraseña"
                            v-model="usuario.password"
                            :rules="rules.required"
                            outlined
                            dense
                            color="green"
                            autocomplete="false"
                            class="mt-4"
                            type="password"
                          >
                          </v-text-field>
                        </v-form>

                        <template>
                          <div>
                            <v-menu
                              ref="menu"
                              v-model="menu"
                              :close-on-content-click="false"
                              transition="scale-transition"
                              offset-y
                              min-width="auto"
                            >
                              <template v-slot:activator="{ on, attrs }">
                                <v-text-field
                                  v-model="usuario.fechanacimiento"
                                  label="Fecha de cumpleaños"
                                  prepend-icon="mdi-calendar"
                                  :rules="rules.required"
                                  readonly
                                  v-bind="attrs"
                                  v-on="on"
                                ></v-text-field>
                              </template>
                              <v-date-picker
                                v-model="usuario.fechanacimiento"
                                :active-picker.sync="activePicker"
                                :max="
                                  new Date(
                                    Date.now() -
                                      new Date().getTimezoneOffset() * 60000
                                  )
                                    .toISOString()
                                    .substr(0, 10)
                                "
                                min="1950-01-01"
                                @change="save()"
                              ></v-date-picker>
                            </v-menu>
                          </div>
                        </template>
                        <v-row>
                          <v-col cols="12" sm="7"> </v-col>
                          <v-col cols="12" sm="7">
                            <v-checkbox
                              required="required"
                              class="mt-n1"
                              color="green"
                              v-model="checkbox"
                              :label="`Términos y condiciones:`"
                            >
                            </v-checkbox>
                          </v-col>
                        </v-row>
                        <v-btn
                          color="green"
                          @click="crearUsuario()"
                          dark
                          block
                          tile
                        >
                          Registrarse
                        </v-btn>
                      </v-col>
                    </v-row>
                  </v-card-text>
                </v-col>
              </v-row>
            </v-window-item>
          </v-window>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import Navbar from "../components/Navigation.vue";
import axios from "axios";
import Swal from "sweetalert2";
export default {
  components: {
    Navbar,
  },
  beforeMount() {
    this.cleanCampos()
  },
  dialogError: false,
  data: () => ({
    checkbox: false,
    activePicker: null,
    menu: false,
    step: 1,
    usuario: {
      nombre: "",
      usuario: "",
      cedula: "",
      password: "",
      correo: "",
      celular: "",
      fechanacimiento: "",
    },
    rules: {
      required: [(v) => !!v || "El campo es obligatorio"],
      min: [(v) => v.length >= 8 || "Min 8 characters"],
      emailRules: [
        (v) => !!v || "El campo es obligatorio",
        (v) =>
          /^[a-zA-Z0-9.]+@([a-z]|[A-Z]).+(..{2,3}){1,2}$/.test(v) ||
          "Correo invalido",
      ],
      texto: [
        (v) => !!v || "El campo es obligatorio",
        (v) => /^[a-zA-ZñÑáéíóú ÁÉÍÓÚ]*$/.test(v) || "Ingrese solo letras",
      ],
      celular: [
        (v) => !!v || "El campo es obligatorio",
        (v) => /^[0-9+]*$/.test(v) || "Ingrese solo numeros",
        (v) => v.length == 10 || "Debe ingresar 10 caracteres",
      ],
      doc: [
        (v) => !!v || "El campo es obligatorio",
        (v) => /^[0-9+]*$/.test(v) || "Ingrese solo numeros",
        (v) => v.length <= 20 || "Max 20 caracteres",
      ],
    },
  }),
  watch: {
    menu(val) {
      val && setTimeout(() => (this.activePicker = "YEAR"));
    },
  },
  methods: {
    save(date) {
      this.$refs.menu.save(date);
    },
    async cleanCampos()
    {
      (this.usuario.usuario = ""),
      (this.usuario.password = "")
    },
    async crearUsuario() {
      if (this.$refs.formRegistro.validate()) {
        try {
          if (this.checkbox == true) {
            console.log(this.checkbox);
            console.log("Inicio guardar usuario");
            let usuario = Object.assign({}, this.usuario);
            console.log(usuario);
            let response = await axios.post(
              "http://localhost:3001/personaCreate",
              usuario
            );
            let resp = response.data;
            if (resp.ok == true) {
              localStorage.setItem("user-id", resp.content.id);
              let token = resp.content.token;
              localStorage.setItem("token", token);
              this.$router.push("/user");
            }
          } else {
            Swal.fire({
              icon: "error",
              title: "Ups...",
              text: "El checkbox es obligatorio",
            });
          }
        } catch (error) {
          this.dialogError = true;
          console.log(error);
        }
      } else {
        Swal.fire({
          icon: "error",
          title: "Ups...",
          text: "Diligencie correctamente el formulario",
        });
      }
    },
    async ingreso() {
      try {
        if (this.$refs.formLogin.validate()) {
          this.dialogError = false;
          let response = await axios.post(
            "http://localhost:3001/login",
            this.usuario
          );
          let usuario = response.data;
          this.cleanCampos();
          if (usuario.ok == true) {
            let rol = usuario.content.rol;
            let id = usuario.content.id;
            localStorage.setItem("user-in", rol);
            localStorage.setItem("user-id", id);
            let token = usuario.content.token;
            localStorage.setItem("token", token);
            if (rol == 0) {
              this.$router.push("/admin");
            } else if (rol == 1) {
              this.$router.push("/collector");
            } else {
              this.$router.push("/user");
            }
          } else {
            this.dialogError = true;
          }
        } else {
          Swal.fire({
            icon: "error",
            title: "Ups...",
            text: "Diligencie correctamente el formulario",
          });
        }
      } catch (error) {
        this.dialogError = true;
      }
    },
    propos: {
      source: String,
    },
  },
};
</script>
<style scoped>
.v-application .rounded-bl-xl {
  border-bottom-left-radius: 300px !important;
}
.v-application .rounded-br-xl {
  border-bottom-right-radius: 300px !important;
}
</style>
