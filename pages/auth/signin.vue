<template>
  <div class="flex items-center justify-center min-h-screen bg-gray-100">
    <div class="px-8 py-6 pb-3 mt-4 text-left bg-white shadow-lg min-w-500">
      <div class="flex justify-center">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="w-20 h-20 text-blue-600"
          fill="none"
          viewBox="0 0 24 24"
          stroke="currentColor"
        >
          <path d="M12 14l9-5-9-5-9 5 9 5z" />
          <path
            d="M12 14l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14z"
          />
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M12 14l9-5-9-5-9 5 9 5zm0 0l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14zm-4 6v-7.5l4-2.222"
          />
        </svg>
      </div>
      <div>
        <div class="mt-4">
          <div>
            <label class="block">Email</label>
            <input
              type="text"
              placeholder="Email"
              class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
              name="email"
              v-model="auth.email"
            />
          </div>
          <div class="mt-4">
            <label class="block">Password</label>
            <input
              type="password"
              placeholder="Password"
              class="w-full px-4 py-2 mt-2 border rounded-md focus:outline-none focus:ring-1 focus:ring-blue-600"
              name="placeholder"
              v-model="auth.password"
            />
          </div>
          <div class="flex items-baseline justify-between">
            <button
              class="px-6 py-2 mt-4 mb-4 text-white bg-blue-600 rounded-lg hover:bg-blue-900"
              @click="validateForm"
            >
              Login
            </button>
          </div>
        </div>
      </div>
      <div
        class="bg-red-100 rounded-lg py-5 px-6 mb-4 text-base text-red-700 mb-3"
        role="alert"
        v-if="loginErrors.show"
      >
        {{ loginErrors.message }}
      </div>
      <div
        class="bg-yellow-100 rounded-lg py-5 px-6 mb-4 text-base text-yellow-700 mb-3"
        role="alert"
        v-if="formErrors.show"
      >
        {{ formErrors.message }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DocPortalesPublicosNuxtSignin",

  data() {
    return {
      loginErrors: {
        message: "",
        show: false,
      },
      formErrors: {
        message: "",
        show: false,
      },
      auth: {
        email: "",
        password: "",
      },
    };
  },

  mounted() {},

  methods: {
    cleanError() {
      this.loginErrors.show = false;
      this.formErrors.show = false;
    },
    validateForm() {
      this.cleanError();
      if (this.auth.email === "") {
        this.formErrors.message = "Email is required";
        this.formErrors.show = true;
        return false;
      }
      if (this.auth.password === "") {
        this.formErrors.message = "Password is required";
        this.formErrors.show = true;
        return false;
      }
      this.login();
    },
    login() {
      let that = this;
      this.$fire.auth
        .signInWithEmailAndPassword(this.auth.email, this.auth.password)
        .catch(function (error) {
          if (error.message.includes("(auth/invalid-email)")) {
            that.loginErrors.message = "Email y/o contraseña incorrectos";
            that.loginErrors.show = true;
          }
        })
        .then((user) => {
          if (user != null) {
            $nuxt.$router.push("/");
          }
        });
    },
  },
};
</script>

<style scoped>
.min-w-500 {
  min-width: 500px;
}
</style>
