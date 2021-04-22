<template>
  <div>
    <section class="min-h-screen bg-white text-gray-900 flex justify-center">
      <div
        class="max-w-screen-xl m-0 sm:m-0 bg-white sm:rounded-lg flex justify-center flex-1"
      >
        <div class="lg:w-1/2 xl:w-5/12 p-6 sm:p-12 mt-20 sm:mt-40">
          <div>
            <img src="../../assets/logo.svg" class="w-72 sm:w-64 mx-auto" />
          </div>
          <div class="mt-12 flex flex-col items-center">
            <h1
              v-if="!isSignedUp"
              class="text-4xl xl:text-4xl text-indigo-600 font-bold"
            >
              Sign Up for Starting Chat
            </h1>
            <h1
              v-if="isSignedUp"
              class="text-4xl xl:text-4xl text-indigo-600 font-bold"
            >
              Sign In to Continue Chat
            </h1>
            <div class="w-full flex-1 mt-8">
              <div class="max-w-full sm:mx-28 mx-10 space-y-4">
                <input
                  v-if="!isSignedUp"
                  v-model="userName"
                  class="w-full px-8 py-4 rounded-lg font-medium bg-gray-100 border border-gray-200 placeholder-gray-500 text-md focus:outline-none focus:border-gray-400 focus:bg-white"
                  type="text"
                  placeholder="Username"
                />
                <input
                  v-if="!isSignedUp"
                  v-model="userUID"
                  class="w-full px-8 py-4 rounded-lg font-medium bg-gray-100 border border-gray-200 placeholder-gray-500 text-md focus:outline-none focus:border-gray-400 focus:bg-white"
                  type="text"
                  placeholder="Unique UID"
                />
                <input
                  v-if="isSignedUp"
                  v-model="userUID"
                  class="w-full px-8 py-4 rounded-lg font-medium bg-gray-100 border border-gray-200 placeholder-gray-500 text-md focus:outline-none focus:border-gray-400 focus:bg-white"
                  type="text"
                  placeholder="Enter Your Unique UID"
                />
                <button
                  v-if="!isSignedUp"
                  @click="createNewUser()"
                  class="mt-5 tracking-wide font-semibold bg-indigo-500 text-gray-100 w-full py-4 rounded-lg hover:bg-indigo-700 transition-all duration-300 ease-in-out flex items-center justify-center focus:shadow-outline focus:outline-none"
                >
                  <svg
                    class="w-10 h-10 -ml-2"
                    fill="none"
                    stroke="currentColor"
                    strokeWidth="2"
                    strokeLinecap="round"
                    strokeLinejoin="round"
                  >
                    <path d="M16 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2" />
                    <circle cx="8.5" cy="7" r="4" />
                    <path d="M20 8v6M23 11h-6" />
                  </svg>
                  <span class="ml-3 text-2xl"> Sign Up </span>
                </button>
                <button
                  v-if="isSignedUp"
                  @click="login()"
                  class="mt-5 tracking-wide font-semibold bg-indigo-500 text-gray-100 w-full py-4 rounded-lg hover:bg-indigo-700 transition-all duration-300 ease-in-out flex items-center justify-center focus:shadow-outline focus:outline-none"
                >
                  <svg
                    class="w-10 h-10 -ml-2"
                    fill="none"
                    stroke="currentColor"
                    strokeWidth="2"
                    strokeLinecap="round"
                    strokeLinejoin="round"
                  >
                    <path d="M16 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2" />
                    <circle cx="8.5" cy="7" r="4" />
                    <path d="M20 8v6M23 11h-6" />
                  </svg>
                  <span class="ml-3 text-2xl"> Sign In </span>
                </button>
                <p
                  @click="toggleScreen()"
                  v-if="!isSignedUp"
                  class="mt-4 text-md text-gray-600 text-center cursor-pointer"
                >
                  Already have an account!
                  <span class="text-indigo-600 font-bold">Sign In</span>
                </p>
                <p
                  @click="toggleScreen()"
                  v-if="isSignedUp"
                  class="mt-4 text-md text-gray-600 text-center cursor-pointer"
                >
                  Not have an account!
                  <span class="text-indigo-600 font-bold">Sign Up</span>
                </p>
                <p class="mt-6 text-sm text-gray-600 text-center">
                  I agree to abide by templatana&apos;s
                  <span class="border-b border-gray-500 border-dotted">
                    Terms of Service
                  </span>
                  and its
                  <span class="border-b border-gray-500 border-dotted">
                    Privacy Policy
                  </span>
                </p>
              </div>
            </div>
          </div>
        </div>
        <div class="flex-1 bg-white text-center hidden lg:flex">
          <div
            class="m-12 xl:m-16 w-full bg-contain bg-center bg-no-repeat"
            style="
              background-image: url('https://storage.googleapis.com/devitary-image-host.appspot.com/15848031292911696601-undraw_designer_life_w96d.svg');
            "
          />
        </div>
      </div>
    </section>
    <loader v-if="showloader" />
  </div>
</template>
<script>
import { CometChat } from "@cometchat-pro/chat";
// import { CometChatAvatar } from "../../cometchat-pro-vue-ui-kit/";

import { COMETCHAT_CONSTANTS } from "../../CONSTS";
import Loader from "../Loader/Loader";

import * as style from "./style";

export default {
  name: "AppHome",
  components: {
    Loader,
  },
  data() {
    return {
      isSignedUp: false,
      userName: "",
      userUID: "",
      users: [
        {
          name: "superhero1",
          image:
            "https://data-us.cometchat.io/assets/images/avatars/ironman.png",
        },
        {
          name: "superhero2",
          image:
            "https://data-us.cometchat.io/assets/images/avatars/captainamerica.png",
        },
        {
          name: "superhero3",
          image:
            "https://data-us.cometchat.io/assets/images/avatars/spiderman.png",
        },
        {
          name: "superhero4",
          image:
            "https://data-us.cometchat.io/assets/images/avatars/wolverine.png",
        },
        {
          name: "superhero5",
          image:
            "https://data-us.cometchat.io/assets/images/avatars/cyclops.png",
        },
      ],
      showloader: false,
      uid: null,
    };
  },
  computed: {
    styles() {
      return {
        wrapper: style.wrapperStyle(),
        title: style.titleStyle(),
        subtitle: style.subtitleStyle(),
        userWrapper: style.userWrapperStyle(),
        userSelector: style.userSelectorStyle(),
        avatarWrapper: style.avatarWrapperStyle(),
        loginText: style.loginTextStyle(),
        input: style.inputStyle(),
        loginButton: style.loginButtonStyle(),
      };
    },
  },
  methods: {
    toggleScreen() {
      this.isSignedUp = !this.isSignedUp
    },
    createNewUser() {
      this.showloader = true;
      // this.userUID = Math.random().toString(36).substr(2, 9);
      let newUser = { uid: this.userUID, name: this.userName };
      let apiKey = "f93f0ea437e8bb81691498a46b8a8b1efc5dc01f";
      CometChat.createUser(newUser, apiKey).then((res) => {
        console.log("Created User", res);
        this.showloader = false;
        this.isSignedUp = true;
      });
    },

    login() {
      if (this.userUID) {
        // this.uid = this.userUID;
      } else {
        alert("Please enter valid UID");
        return;
      }
      this.showloader = true;
      CometChat.login(this.userUID, COMETCHAT_CONSTANTS.AUTH_KEY)
        .then((user) => {
          console.log("User logged in : ", user);
          location.href = "#/user-screen";
        })
        .catch((error) => {
          console.log("Error in [login]", error);
          alert((error || {}).message || "Error in login");
        })
        .finally(() => {
          this.showloader = false;
        });
    },
  },
  created() {
    this.showloader = true;
    CometChat.getLoggedinUser().then(
      (user) => {
        console.log("user", user);
        if (user) {
          location.href = "#/user-screen";
          this.uid = user.getUid();
        } else {
          this.showloader = false;
        }
      },
      (error) => {
        console.log("Error in [getLoggedinUser]", error);
        this.showloader = false;
      }
    );
  },
};
</script>