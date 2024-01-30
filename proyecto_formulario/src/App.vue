<template>
  <div>
    <b-navbar type="dark" variant="dark">
      <b-navbar-nav>
        <b-nav-item href="#">Formulario de registro</b-nav-item>

        <!-- Navbar dropdowns -->
        <b-nav-item-dropdown text="Lang" right>
          <b-dropdown-item href="#">EN</b-dropdown-item>
          <b-dropdown-item href="#">ES</b-dropdown-item>
          <b-dropdown-item href="#">RU</b-dropdown-item>
          <b-dropdown-item href="#">FA</b-dropdown-item>
        </b-nav-item-dropdown>

        <b-nav-item-dropdown text="User" right>
          <b-dropdown-item href="#">Account</b-dropdown-item>
          <b-dropdown-item href="#">Settings</b-dropdown-item>
        </b-nav-item-dropdown>
      </b-navbar-nav>
    </b-navbar>

    <!-- AQUI INICIO FORMULARIOOOOOOO -->
    <b-form @submit="onSubmit" @reset="onReset" v-if="show" class="form-container">
      <!-- Nombre -->
      <b-form-group
        id="input-group-1"
        label="Nombre completo:"
        label-for="input-1"
        :state="!isValid('name')"
      >
        <b-form-input
          id="input-1"
          v-model="form.name"
          placeholder="Nombre"
          required
        ></b-form-input>
        <div class="invalid-feedback">Por favor, ingrese su nombre.</div>
      </b-form-group>

      <!-- Apellido Paterno -->
      <b-form-group
        id="input-group-2"
        label="Apellido Paterno:"
        label-for="input-2"
        :state="!isValid('surname')"
      >
        <b-form-input
          id="input-2"
          v-model="form.surname"
          placeholder="Apellido Paterno"
          required
        ></b-form-input>
        <div class="invalid-feedback">
          Por favor, ingrese su apellido paterno.
        </div>
      </b-form-group>

      <!-- Dirección -->
      <b-form-group
        id="input-group-4"
        label="Dirección (CP, Calle, número, Ciudad):"
        label-for="input-4"
        :state="!isValid('address')"
      >
        <b-form-input
          id="input-4"
          v-model="form.address"
          placeholder="Dirección"
          required
        ></b-form-input>
        <div class="invalid-feedback">Por favor, ingrese su dirección.</div>
      </b-form-group>

      <!-- Fecha de nacimiento -->
      <b-form-group
        id="input-group-5"
        label="Fecha de nacimiento:"
        label-for="input-5"
        :state="!isValid('birthDate')"
      >
        <b-form-input
          id="input-5"
          v-model="form.birthDate"
          type="date"
          required
        ></b-form-input>
        <div class="invalid-feedback">Debe tener al menos 18 años.</div>
      </b-form-group>

      <!-- Correo electrónico -->
      <b-form-group
        id="input-group-6"
        label="Correo electrónico:"
        label-for="input-6"
        :state="!isValid('email')"
      >
        <b-form-input
          id="input-6"
          v-model="form.email"
          type="email"
          placeholder="Correo electrónico"
          required
        ></b-form-input>
        <div class="invalid-feedback">
          Por favor, ingrese un correo electrónico válido.
        </div>
      </b-form-group>

      <!-- Número telefónico -->
      <b-form-group
        id="input-group-7"
        label="Número telefónico:"
        label-for="input-7"
        :state="!isValid('phoneNumber')"
      >
        <b-form-input
          id="input-7"
          v-model="form.phoneNumber"
          type="tel"
          placeholder="Número telefónico"
          required
        ></b-form-input>
        <div class="invalid-feedback">
          Por favor, ingrese un número telefónico válido.
        </div>
      </b-form-group>

      <!-- Fotografía -->
      <b-form-group
        id="input-group-8"
        label="Fotografía (PNG):"
        label-for="input-8"
        :state="!isValid('photo')"
      >
        <b-form-file
          id="input-8"
          v-model="form.photo"
          accept=".png ,.jpg"
          required
        ></b-form-file>
        <div class="invalid-feedback">
          Por favor, cargue una fotografía en formato PNG (menos de 3 MB).
        </div>
      </b-form-group>

      <b-button type="submit" variant="primary">Enviar</b-button>
      <b-button type="reset" variant="danger">Restablecer</b-button>
    </b-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      form: {
        name: "",
        surname: "",
        address: "",
        birthDate: "",
        email: "",
        phoneNumber: "",
        photo: null,
      },
      show: true,
    };
  },
  methods: {
    onSubmit(event) {
      event.preventDefault();
      if (this.isFormValid()) {
        // Envía el formulario
        alert(JSON.stringify(this.form));
      } else {
        // Muestra un mensaje de error o toma otras acciones
        console.log("El formulario tiene algunos errores.");
        const errorMessage = this.getErrorMessage();
        alert(`Error ${errorMessage}`);
      }
    },
    getErrorMessage() {
      const errorFields = Object.keys(this.form).filter(
        (field) => !this.isValid(field)
      );
      if (errorFields.length > 0) {
        return `Por favor, complete los campos obligatorios y corrija los errores en los siguientes campos: ${errorFields.join(
          ", "
        )}.`;
      } else {
        return "El formulario contiene errores.";
      }
    },
    onReset(event) {
      event.preventDefault();
      // Restablece el formulario
      this.form = {
        name: "",
        surname: "",
        address: "",
        birthDate: "",
        email: "",
        phoneNumber: "",
        photo: null,
      };
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
    isValid(field) {
      // Realiza la validación manualmente para cada campo
      switch (field) {
        case "name":
          return this.form.name.trim() !== "";
        case "surname":
          return this.form.surname.trim() !== "";
        case "address":
          return this.form.address.trim() !== "";
        case "birthDate":
          // Validación de la fecha de nacimiento
          return this.calculateAge(this.form.birthDate) >= 18;
        case "email":
          // Validación de correo electrónico mediante una expresión regular simple
          const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
          return emailRegex.test(this.form.email);
        case "phoneNumber":
          // Validación del número telefónico por longitud
          return this.form.phoneNumber.length === 10;
        case "photo":
          // Validación de la presencia de una foto
          return this.form.photo !== null;
        default:
          return true;
      }
    },
    isFormValid() {
      // Verifica si todos los campos son válidos
      return Object.keys(this.form).every((field) => this.isValid(field));
    },
    calculateAge(birthDate) {
      const today = new Date();
      const birthDateObj = new Date(birthDate);
      const age = today.getFullYear() - birthDateObj.getFullYear();
      const monthDiff = today.getMonth() - birthDateObj.getMonth();
      return (
        age -
        (monthDiff < 0 ||
        (monthDiff === 0 && today.getDate() < birthDateObj.getDate())
          ? 1
          : 0)
      );
    },
  },
};
</script>

<style>

/* Estilos para centrar y limitar el ancho del formulario */
.form-container {
  max-width: 600px; /* Ajusta este valor según tus preferencias */
  margin: 0 auto; /* Centra horizontalmente */
  background-color: #f5f5f5; /* Color de fondo gris */
  padding: 20px; /* Ajusta el relleno según sea necesario */
}
/* Estilos generales */
.invalid-feedback {
  color: #dc3545;
  font-size: 80%;
}

/* Estilos para pantallas más pequeñas */
@media (max-width: 768px) {
  .b-form-group {
    margin-bottom: 1.5rem;
  }

  .b-form-group label {
    display: block;
    margin-bottom: 0.5rem;
  }

  .b-form-input,
  .b-form-file {
    width: 100%;
  }

  .b-button {
    width: 100%;
    margin-top: 1rem;
  }

  

  
}
</style>