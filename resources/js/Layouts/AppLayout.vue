<script setup>
import { ref, watch, onMounted } from "vue";
import { usePage } from "@inertiajs/vue3";
import ApplicationLogo from "@/Components/ApplicationLogo.vue";
import Dropdown from "@/Components/Dropdown.vue";
import DropdownLink from "@/Components/DropdownLink.vue";
import NavLink from "@/Components/NavLink.vue";
import ResponsiveNavLink from "@/Components/ResponsiveNavLink.vue";
import { Link, Head } from "@inertiajs/vue3";

defineProps({
  title: String,
});

const showingNavigationDropdown = ref(false);

// Simple toast for global success/error flashes
const page = usePage();
const toast = ref({ type: null, message: null, visible: false });
let toastTimer = null;

const showToast = (type, message) => {
  if (!message) return;
  toast.value = { type, message, visible: true };
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => {
    toast.value.visible = false;
  }, 3500);
};

watch(
  () => page.props.flash,
  (flash) => {
    if (flash?.success) showToast("success", flash.success);
    if (flash?.error) showToast("error", flash.error);
  },
  { deep: true, immediate: true }
);
</script>

<template>
  <div>
    <!-- Set App Icon as Favicon if available -->
    <Head>
      <link
        v-if="$page.props.appSettings && $page.props.appSettings.app_icon"
        rel="icon"
        type="image/x-icon"
        :href="`/storage/${$page.props.appSettings.app_icon}`"
      />
    </Head>

    <div class="min-h-screen bg-gradient-to-br from-gray-50 via-white to-teal-50">
      <nav class="bg-gradient-to-r from-teal-50 via-emerald-50 to-cyan-50 border-b border-teal-200/40 shadow-sm">
        <!-- Primary Navigation Menu -->
        <div class="mx-auto max-w px-3 sm:px-6 lg:px-8">
          <div class="flex h-14 sm:h-16 justify-between items-center">
            <div class="flex">
              <!-- Logo - Uses App Settings if available, otherwise Company Info -->
              <div class="flex shrink-0 items-center gap-2 sm:gap-3">
                <Link
                  :href="route('dashboard')"
                  class="flex items-center gap-2 sm:gap-3 hover:opacity-80 transition-opacity duration-200"
                >
                  <!-- App Logo (from App Settings) takes priority -->
                  <img
                    v-if="$page.props.appSettings && $page.props.appSettings.app_logo"
                    :src="`/storage/${$page.props.appSettings.app_logo}`"
                    alt="App Logo"
                    class="block h-8 sm:h-10 w-auto"
                  />
                  <!-- Fallback to Company Logo -->
                  <img
                    v-else-if="$page.props.companyInfo && $page.props.companyInfo.logo"
                    :src="`/storage/${$page.props.companyInfo.logo}`"
                    alt="Company Logo"
                    class="block h-8 sm:h-10 w-auto"
                  />
                  <!-- Final fallback to default ApplicationLogo -->
                  <!-- <ApplicationLogo
                                        v-else
                                        class="block h-9 w-auto fill-current text-white"
                                    />
                                     -->
                  <!-- App Name (from App Settings) takes priority over Company Name -->
                  <span
                    v-if="$page.props.appSettings && $page.props.appSettings.app_name"
                    class="text-base sm:text-xl font-bold bg-gradient-to-r from-teal-600 to-emerald-600 bg-clip-text text-transparent"
                  >
                    {{ $page.props.appSettings.app_name }}
                  </span>
                  <span
                    v-else-if="
                      $page.props.companyInfo && $page.props.companyInfo.company_name
                    "
                    class="text-base sm:text-xl font-bold bg-gradient-to-r from-teal-600 to-emerald-600 bg-clip-text text-transparent"
                  >
                    {{ $page.props.companyInfo.company_name }}
                  </span>
                </Link>
              </div>

              <!-- Navigation Links -->
              <!-- <div class="hidden space-x-8 sm:ms-12 sm:flex sm:items-center">
                                <NavLink
                                    :href="route('dashboard')"
                                    :active="route().current('dashboard')"
                                >
                                    Dashboard
                                </NavLink>
                            </div> -->
            </div>

            <div class="hidden sm:ms-4 sm:flex sm:items-center gap-2 sm:gap-3">
              <!-- User Profile Display -->
              <div
                class="inline-flex items-center gap-2 h-10 px-3 rounded-lg border border-teal-200/50 bg-gradient-to-r from-teal-50/50 to-emerald-50/50 shadow-sm"
              >
                <div
                  class="flex items-center justify-center w-7 h-7 rounded-full bg-gradient-to-br from-teal-500 to-emerald-600 text-white font-bold text-xs shadow-sm"
                >
                  {{ $page.props.auth.user.name.charAt(0).toUpperCase() }}
                </div>
                <div class="flex flex-col justify-center">
                  <span class="text-xs font-semibold text-gray-800 leading-tight">
                    {{ $page.props.auth.user.name }}
                  </span>
                  <span class="text-[10px] text-gray-500 leading-tight">Logged in</span>
                </div>
              </div>

              <!-- POS Button -->
              <Link
                :href="route('sales.index')"
                class="inline-flex items-center justify-center gap-1.5 h-10 px-4 rounded-lg border border-teal-600 bg-gradient-to-r from-teal-600 to-emerald-600 text-xs font-semibold text-white shadow-sm transition-all duration-200 hover:from-teal-700 hover:to-emerald-700 hover:shadow-md hover:-translate-y-0.5 focus:outline-none focus:ring-2 focus:ring-teal-500 focus:ring-offset-1 min-h-[44px] sm:min-h-0"
              >
                <span class="text-base">🏪</span>
                <span>POS</span>
              </Link>

              <!-- Logout Button -->
              <Link
                :href="route('logout')"
                method="post"
                as="button"
                class="inline-flex items-center justify-center gap-1.5 h-10 px-4 rounded-lg border border-gray-300 bg-white text-xs font-semibold text-gray-700 shadow-sm transition-all duration-200 hover:bg-gray-50 hover:border-gray-400 hover:shadow-md hover:-translate-y-0.5 focus:outline-none focus:ring-2 focus:ring-gray-400 focus:ring-offset-1 min-h-[44px] sm:min-h-0"
              >
                <svg
                  class="w-3.5 h-3.5"
                  fill="none"
                  stroke="currentColor"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"
                  ></path>
                </svg>
                <span>Logout</span>
              </Link>
            </div>

            <!-- Hamburger -->
            <div class="-me-2 flex items-center sm:hidden">
              <button
                @click="showingNavigationDropdown = !showingNavigationDropdown"
                class="inline-flex items-center justify-center rounded-lg p-2 text-gray-600 transition-all duration-200 hover:bg-teal-50 hover:text-teal-700 focus:bg-teal-50 focus:text-teal-700 focus:outline-none focus:ring-2 focus:ring-teal-500 min-h-[44px] min-w-[44px]"
              >
                <svg
                  class="h-6 w-6"
                  stroke="currentColor"
                  fill="none"
                  viewBox="0 0 24 24"
                >
                  <path
                    :class="{
                      hidden: showingNavigationDropdown,
                      'inline-flex': !showingNavigationDropdown,
                    }"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M4 6h16M4 12h16M4 18h16"
                  />
                  <path
                    :class="{
                      hidden: !showingNavigationDropdown,
                      'inline-flex': showingNavigationDropdown,
                    }"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M6 18L18 6M6 6l12 12"
                  />
                </svg>
              </button>
            </div>
          </div>
        </div>

        <!-- Responsive Navigation Menu -->
        <div
          :class="{
            block: showingNavigationDropdown,
            hidden: !showingNavigationDropdown,
          }"
          class="sm:hidden border-t border-gray-200/50 bg-white/95 backdrop-blur-sm"
        >
          <div class="space-y-1 pb-2 pt-2">
            <ResponsiveNavLink
              :href="route('dashboard')"
              :active="route().current('dashboard')"
            >
              Dashboard
            </ResponsiveNavLink>
          </div>

          <!-- Responsive Settings Options -->
          <div class="border-t border-gray-200/50 pb-2 pt-3 bg-gradient-to-r from-teal-50/30 to-emerald-50/30">
            <div class="px-4">
              <div class="text-sm font-semibold text-gray-800">
                {{ $page.props.auth.user.name }}
              </div>
              <div class="text-xs font-medium text-gray-600">
                {{ $page.props.auth.user.email }}
              </div>
            </div>

            <div class="mt-2 space-y-1">
              <ResponsiveNavLink :href="route('profile.edit')">
                Profile
              </ResponsiveNavLink>
              <ResponsiveNavLink :href="route('logout')" method="post" as="button">
                Log Out
              </ResponsiveNavLink>
            </div>
          </div>
        </div>
      </nav>

      <!-- Page Content -->
      <main>
        <slot />
      </main>

      <!-- Global Toast -->
      <div
        v-if="toast.visible"
        class="fixed top-6 right-6 z-50 max-w-sm w-full shadow-lg rounded-md border p-4 flex items-start gap-3"
        :class="toast.type === 'success' ? 'bg-green-50 border-green-200 text-green-800' : 'bg-red-50 border-red-200 text-red-800'"
      >
        <span class="text-xl">{{ toast.type === 'success' ? '✅' : '⚠️' }}</span>
        <div class="text-sm leading-5">{{ toast.message }}</div>
        <button class="ml-auto text-gray-500 hover:text-gray-700" @click="toast.visible = false">✖</button>
      </div>

      <!-- App Footer (if configured in App Settings) -->
      <footer
        v-if="$page.props.appSettings && $page.props.appSettings.app_footer"
        class="bg-secondary border-t border-gray-700 py-4 mt-8"
      >
        <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
          <p class="text-center text-sm text-gray-400">
            {{ $page.props.appSettings.app_footer }}
          </p>
        </div>
      </footer>
    </div>
  </div>
</template>
