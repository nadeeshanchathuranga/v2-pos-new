<script setup>
/**
 * Dashboard Component Script
 *
 * Main dashboard for POS system users
 * Uses AppLayout for consistent navigation
 */
import AppLayout from "@/Layouts/AppLayout.vue";
import { Head, Link, usePage, router } from "@inertiajs/vue3";
import { computed, ref, onMounted } from "vue";

const page = usePage();
const pageTitle = computed(() => {
  const appName = page.props.appSettings?.app_name || "Online මුදලාලි";
  return appName;
});

// Set default tab based on user role
const getDefaultTab = () => {
  const userRole = page.props.auth.user.role;
  if ([0, 1, 3].includes(userRole)) {
    return "products"; // Products section for these roles
  }
  return "shops"; // Default to shops for other roles
};

// Track active tab
const activeTab = ref(getDefaultTab());

// Switch tabs and persist selection
const setActiveTab = (tab) => {
  activeTab.value = tab;
};

// Set default tab on mount
onMounted(() => {
  const savedTab = localStorage.getItem("activeTab");
  const fromNavigation = sessionStorage.getItem("fromNavigation");

  if (savedTab && fromNavigation === "true") {
    activeTab.value = savedTab;
    sessionStorage.removeItem("fromNavigation");
  } else {
    activeTab.value = getDefaultTab();
    localStorage.removeItem("activeTab");
  }
});
</script>

<template>
  <!-- Page Title for Browser Tab -->
  <Head :title="pageTitle" />

  <AppLayout>
    <div class="min-h-screen bg-gradient-to-br from-gray-50 to-gray-100 p-3 sm:p-4 md:p-6">
      <!-- Header -->
      <div class="mb-4 sm:mb-5">
        <h1 class="text-xl sm:text-2xl md:text-3xl font-bold bg-gradient-to-r from-teal-600 to-emerald-600 bg-clip-text text-transparent mb-1">Online මුදලාලි Dashboard</h1>
        <p class="text-xs sm:text-sm text-gray-600">Manage your products, stores, and shops</p>
      </div>

      <!-- Tab Navigation -->
      <div class="mb-6 sm:mb-8 flex justify-center overflow-x-auto">
        <div class="inline-flex gap-1.5 sm:gap-2 bg-white rounded-xl p-1.5 sm:p-2 border border-gray-200 shadow-lg">

          <button
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            @click="setActiveTab('products')"
            :class="[
              'flex items-center gap-1.5 sm:gap-2 px-3 sm:px-5 py-2.5 sm:py-3 rounded-lg font-medium text-xs sm:text-sm transition-all duration-300 whitespace-nowrap min-h-[44px]',
              activeTab === 'products'
                ? 'bg-gradient-to-r from-teal-600 to-emerald-600 text-white shadow-md transform scale-105'
                : 'text-gray-700 hover:bg-teal-50 hover:text-teal-700 active:bg-teal-100',
            ]"
          >
            <span class="text-base sm:text-lg">📦</span>
            <span>Products</span>
          </button>

          <button
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            @click="setActiveTab('stores')"
            :class="[
              'flex items-center gap-1.5 sm:gap-2 px-3 sm:px-5 py-2.5 sm:py-3 rounded-lg font-medium text-xs sm:text-sm transition-all duration-300 whitespace-nowrap min-h-[44px]',
              activeTab === 'stores'
                ? 'bg-gradient-to-r from-teal-600 to-emerald-600 text-white shadow-md transform scale-105'
                : 'text-gray-700 hover:bg-teal-50 hover:text-teal-700 active:bg-teal-100',
            ]"
          >
            <span class="text-base sm:text-lg">🛒</span>
            <span>Stores</span>
          </button>

          <button
            @click="setActiveTab('shops')"
            :class="[
              'flex items-center gap-1.5 sm:gap-2 px-3 sm:px-5 py-2.5 sm:py-3 rounded-lg font-medium text-xs sm:text-sm transition-all duration-300 whitespace-nowrap min-h-[44px]',
              activeTab === 'shops'
                ? 'bg-gradient-to-r from-teal-600 to-emerald-600 text-white shadow-md transform scale-105'
                : 'text-gray-700 hover:bg-teal-50 hover:text-teal-700 active:bg-teal-100',
            ]"
          >
            <span class="text-base sm:text-lg">💰</span>
            <span>Shops</span>
          </button>

          <button
            v-if="[0, 1, 2, 3].includes($page.props.auth.user.role)"
            @click="setActiveTab('reports')"
            :class="[
              'flex items-center gap-1.5 sm:gap-2 px-3 sm:px-5 py-2.5 sm:py-3 rounded-lg font-medium text-xs sm:text-sm transition-all duration-300 whitespace-nowrap min-h-[44px]',
              activeTab === 'reports'
                ? 'bg-gradient-to-r from-teal-600 to-emerald-600 text-white shadow-md transform scale-105'
                : 'text-gray-700 hover:bg-teal-50 hover:text-teal-700 active:bg-teal-100',
            ]"
          >
            <span class="text-base sm:text-lg"> </span>
            <span>Reports</span>
          </button>

          <!-- <button
            v-if="[0].includes($page.props.auth.user.role)"
            @click="setActiveTab('system')"
            :class="[
              'flex items-center gap-2 px-5 py-2.5 rounded-md font-medium text-sm transition-all duration-200',
              activeTab === 'system'
                ? 'bg-blue-600 text-white'
                : 'text-gray-700 hover:bg-gray-50',
            ]"
          >
            <span class="text-lg">⚙️</span>
            <span>System</span>
          </button> -->

          <button
            v-if="![2, 3].includes($page.props.auth.user.role)"
            @click="setActiveTab('settings')"
            :class="[
              'flex items-center gap-1.5 sm:gap-2 px-3 sm:px-5 py-2.5 sm:py-3 rounded-lg font-medium text-xs sm:text-sm transition-all duration-300 whitespace-nowrap min-h-[44px]',
              activeTab === 'settings'
                ? 'bg-gradient-to-r from-teal-600 to-emerald-600 text-white shadow-md transform scale-105'
                : 'text-gray-700 hover:bg-teal-50 hover:text-teal-700 active:bg-teal-100',
            ]"
          >
            <span class="text-base sm:text-lg">🔧</span>
            <span>Settings</span>
          </button>
        </div>
      </div>

      <!-- Products Section -->
      <div
        v-if="activeTab === 'products' && [0, 1, 3].includes($page.props.auth.user.role)"
        class="bg-white rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-gray-100 shadow-xl"
      >
        <h3
          class="text-lg sm:text-xl font-semibold bg-gradient-to-r from-teal-700 to-emerald-700 bg-clip-text text-transparent mb-4 pb-3 border-b-2 border-teal-100 flex items-center gap-2"
        >
          <span class="text-2xl">📦</span> Inventory Management
        </h3>
        <div class="grid gap-3 sm:gap-4 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5">
          <Link
            v-if="[0, 1 ,3].includes($page.props.auth.user.role)"
            :href="route('products.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📦</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Products</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage the products & information.</div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('brands.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🏷️</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Brands</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage brands</div>
          </Link>

          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('categories.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📂</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Categories</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage categories</div>
          </Link>

          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('types.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🔹</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Types</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage types</div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('measurement-units.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📏</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Units</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Measurement units</div>
          </Link>

          <Link
            v-if="[0, 1 ,3].includes($page.props.auth.user.role)"
            :href="route('suppliers.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🏭</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Suppliers</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage suppliers</div>
          </Link>
        </div>
      </div>

      <!-- Stores Section -->
      <div
        v-if="activeTab === 'stores' && [0, 1, 3, 3].includes($page.props.auth.user.role)"
        class="bg-white rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-gray-100 shadow-xl"
      >
        <h3
          class="text-lg sm:text-xl font-semibold bg-gradient-to-r from-teal-700 to-emerald-700 bg-clip-text text-transparent mb-4 pb-3 border-b-2 border-teal-100 flex items-center gap-2"
        >
          <span class="text-2xl">🛍️</span> Stores
        </h3>
        <div class="grid gap-3 sm:gap-4 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5">
          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('purchase-order-requests.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📋</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Purchase Order Requests
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage the purchase orders</div>
          </Link>

          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('good-receive-notes.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📦</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Goods Received Notes
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">
              Track the received goods from the purchase orders.
            </div>
          </Link>

          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('good-receive-note-returns.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📦</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Goods Return Notes</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">
              Track the return goods from the purchase orders.
            </div>
          </Link>

          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('product-release-notes.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📝</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Goods Transfer Release Notes
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">
              Manage goods transfers from stores to shop.
            </div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('purchase-expenses.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">💸</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Supplier Payments</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Track the supplier payments</div>
          </Link>

        </div>
      </div>

      <!-- Shops Section -->
      <div
        v-if="activeTab === 'shops'"
        class="bg-white rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-gray-100 shadow-xl"
      >
        <h3
          class="text-lg sm:text-xl font-semibold bg-gradient-to-r from-teal-700 to-emerald-700 bg-clip-text text-transparent mb-4 pb-3 border-b-2 border-teal-100 flex items-center gap-2"
        >
          <span class="text-2xl">💰</span> Sales Management
        </h3>
        <div class="grid gap-3 sm:gap-4 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5">
          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('customers.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">👥</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Customers</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage customers</div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('discounts.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🏷️</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Discounts</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage discounts</div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('taxes.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📊</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Taxes</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage tax rates</div>
          </Link>

          <Link
            v-if="[0, 1, 2].includes($page.props.auth.user.role)"
            :href="route('sales.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">💳</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Sales</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage sales transactions</div>
          </Link>

          <Link
            v-if="[0, 1, 2].includes($page.props.auth.user.role)"
            :href="route('sales.all')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📜</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Sales History</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">View all sales records</div>
          </Link>

          <Link
            v-if="[0, 1, 2].includes($page.props.auth.user.role)"
            :href="route('quotations.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📄</div>

            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Quotations</div>

            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Create and manage quotations</div>
          </Link>

          <Link
            v-if="[0, 1, 2].includes($page.props.auth.user.role)"
            :href="route('quotation.edit')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">✏️</div>

            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Edit Quotations</div>

            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">View, update and manage quotations</div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('product-transfer-requests.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📤</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Products Transfer Request
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">
              Manage the Products transfer request from shop.
            </div>
          </Link>

          <a
            v-if="[0, 1].includes($page.props.auth.user.role)"
            href="/stock-transfer-returns"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px] block"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🔄</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Products Transfer Returns
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">
              Manage the transfer from shop to store.
            </div>
          </a>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('return.index')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">↩️</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Sales Return</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage sales returns</div>
          </Link>
        </div>
      </div>

      <!-- Report Management -->
      <div
        v-if="
          activeTab === 'reports' && [0, 1, 2, 3].includes($page.props.auth.user.role)
        "
        class="bg-white rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-gray-100 shadow-xl"
      >
        <h3
          class="text-lg sm:text-xl font-semibold bg-gradient-to-r from-teal-700 to-emerald-700 bg-clip-text text-transparent mb-4 pb-3 border-b-2 border-teal-100 flex items-center gap-2"
        >
          <span class="text-2xl"> </span> Report Management
        </h3>
        <div class="grid gap-3 sm:gap-4 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5">
          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('reports.low-stock-shop')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🏪</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Daily Sales Summery
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Products low in shop</div>
          </Link>
          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('reports.low-stock-shop')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🏪</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Shop Low Stock Report
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Products low in shop</div>
          </Link>
          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('reports.low-stock-store')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🏬</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Store Low Stock Report
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Products low in store</div>
          </Link>

          <Link
            v-if="[0, 1, 2, 3].includes($page.props.auth.user.role)"
            :href="route('reports.stock')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📈</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Stock Report</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Current inventory status</div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('reports.expenses')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">💸</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Supplier Payment Report
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Supplier payment details & summary</div>
          </Link>

          <Link
            v-if="[0, 1, 2].includes($page.props.auth.user.role)"
            :href="route('reports.sales-income')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">💰</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Sales Income Report
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Sales income & returns transactions</div>
          </Link>
          <!-- <Link
            v-if="[0, 1,3].includes($page.props.auth.user.role)"
            :href="route('reports.product-release')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div
              class="text-4xl mb-3"
            >
              📦
            </div>
            <div class="font-semibold text-lg text-gray-800 mb-1">Goods Transfer Report</div>
            <div class="text-sm text-gray-600">Release notes report</div>
          </Link>

          <Link
            v-if="[0, 1,3].includes($page.props.auth.user.role)"
            :href="route('reports.stock-transfer-return')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div
              class="text-4xl mb-3"
            >
              🔄
            </div>
            <div class="font-semibold text-lg text-gray-800 mb-1">Goods Transfer Return Report</div>
            <div class="text-sm text-gray-600">Transfer return report</div>
          </Link>


          <Link
            v-if="[0, 1,3].includes($page.props.auth.user.role)"
            :href="route('reports.grn')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div
              class="text-4xl mb-3"
            >
              📥
            </div>
            <div class="font-semibold text-lg text-gray-800 mb-1">
              Goods Received Report
            </div>
            <div class="text-sm text-gray-600">All inbound receipts and totals</div>
          </Link>
          <Link
            v-if="[0, 1,3].includes($page.props.auth.user.role)"
            :href="route('reports.grn-returns')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div
              class="text-4xl mb-3"
            >
              ↩️
            </div>
            <div class="font-semibold text-lg text-gray-800 mb-1">
              Goods Return Report
            </div>
            <div class="text-sm text-gray-600">Returned receipts and quantities</div>
          </Link> -->
          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('reports.product-movements')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🔀</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Product Movement Report
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Track inbound/outbound stock flows</div>
          </Link>

          <Link
            v-if="[0, 1, 3].includes($page.props.auth.user.role)"
            :href="route('reports.product-movement-sales-optimization')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📈</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Sales Optimization Report
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Product movement based sales insights</div>
          </Link>

          <Link
            v-if="[0].includes($page.props.auth.user.role)"
            :href="route('reports.activity-log')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">📝</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Activity Log</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">User activity & audit trail</div>
          </Link>

          <Link
            v-if="[0].includes($page.props.auth.user.role)"
            :href="route('reports.sync')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🔄</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Sync Report</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">View sync activity logs</div>
          </Link>
        </div>
      </div>

      <!-- System Management
      <div
        v-if="activeTab === 'system' && [0, 1].includes($page.props.auth.user.role)"
        class="bg-white rounded-lg p-6 border border-gray-200"
      >
        <h3
          class="text-xl font-semibold text-gray-800 mb-4 pb-3 border-b border-gray-200 flex items-center gap-2"
        >
          <span>⚙️</span> System Management
        </h3>
        <div class="grid gap-4 md:grid-cols-2 lg:grid-cols-5">
          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('users.index')"
            class="group bg-white hover:bg-gray-50 p-4 rounded-lg border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div class="text-4xl mb-3">👤</div>
            <div class="font-semibold text-lg text-gray-800 mb-1">Users</div>
            <div class="text-sm text-gray-600">Manage system users</div>
          </Link>
        </div>
      </div> -->

      <!-- Settings -->
      <div
        v-if="activeTab === 'settings' && ![1, 2, 3].includes($page.props.auth.user.role)"
        class="bg-white rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-gray-100 shadow-xl"
      >
        <h3
          class="text-lg sm:text-xl font-semibold bg-gradient-to-r from-teal-700 to-emerald-700 bg-clip-text text-transparent mb-4 pb-3 border-b-2 border-teal-100 flex items-center gap-2"
        >
          <span class="text-2xl">🔧</span> Settings
        </h3>
        <div class="grid gap-3 sm:gap-4 grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-5">
          <Link
            :href="route('settings.company')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🏢</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">
              Company Information
            </div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Company information & settings</div>
          </Link>

          <Link
            v-if="[0, 1].includes($page.props.auth.user.role)"
            :href="route('users.index')"
            class="group bg-white hover:bg-gray-50 p-4 rounded-lg border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div class="text-4xl mb-3">👤</div>
            <div class="font-semibold text-lg text-gray-800 mb-1">Users</div>
            <div class="text-sm text-gray-600">Manage system users</div>
          </Link>

          <Link
            :href="route('settings.app')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">⚙️</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">App Settings</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">
              Application preferences & configuration
            </div>
          </Link>
          <!-- <Link
            :href="route('settings.smtp')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div
              class="text-4xl mb-3"
            >
              📧
            </div>
            <div class="font-semibold text-lg text-gray-800 mb-1">SMTP Settings</div>
            <div class="text-sm text-gray-600">Email server configuration</div>
          </Link>-->
          <Link
            :href="route('settings.sync')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🔄</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Sync Setting</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Synchronization configuration</div>
          </Link>
          <!-- <Link
            :href="route('settings.bill')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div
              class="text-4xl mb-3"
            >
              🧾
            </div>
            <div class="font-semibold text-lg text-gray-800 mb-1">Bill Setting</div>
            <div class="text-sm text-gray-600">Bill logo, company info, print size</div>
          </Link> -->
          <Link
            v-if="![1].includes($page.props.auth.user.role)"
            :href="route('backup.settings')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🧾</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Database Backup</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Bill logo, company info, print size</div>
          </Link>
          <!-- <Link
          <Link
            :href="route('backup.settings')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div class="text-4xl mb-3">🧾</div>
            <div class="font-semibold text-lg text-gray-800 mb-1">Database Backup</div>
            <div class="text-sm text-gray-600">Bill logo, company info, print size</div>
          </Link>
          <!-- <Link
            :href="route('settings.sync')"
            class="group bg-white hover:bg-gray-50 p-6 rounded-xl border border-gray-200 hover:border-gray-300 transition-all duration-200"
          >
            <div
              class="text-4xl mb-3"
            >
              🔄
            </div>
            <div class="font-semibold text-lg text-gray-800 mb-1">Sync Setting</div>
            <div class="text-sm text-gray-600">Configure sync options</div>
          </Link> -->
          <Link
            :href="route('settings.bill')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🧾</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Bill Settings</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Configure bill options</div>
          </Link>
          <Link
            :href="route('import-export')"
            class="group bg-gradient-to-br from-white to-gray-50 hover:from-teal-50 hover:to-emerald-50 p-5 sm:p-6 rounded-xl border-2 border-gray-200 hover:border-teal-400 shadow-md hover:shadow-2xl transition-all duration-300 cursor-pointer transform hover:-translate-y-1 active:scale-95 min-h-[120px] sm:min-h-[140px]"
          >
            <div class="text-3xl sm:text-4xl mb-3 group-hover:scale-110 transition-transform duration-300">🔄</div>
            <div class="font-semibold text-base sm:text-lg text-gray-800 mb-1 group-hover:text-teal-700">Import & Export</div>
            <div class="text-xs sm:text-sm text-gray-600 group-hover:text-gray-700">Manage data import and export</div>
          </Link>
        </div>
      </div>
    </div>
  </AppLayout>
</template>

<style scoped>
/* Smooth transitions and touch-friendly interactions */
a,
button {
  transition-property: all;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 300ms;
  -webkit-tap-highlight-color: transparent;
}

/* Ensure touch targets are large enough */
button {
  min-height: 44px;
}

/* Smooth scrolling for tab navigation on mobile */
@media (max-width: 640px) {
  .overflow-x-auto {
    scrollbar-width: none;
    -ms-overflow-style: none;
  }
  
  .overflow-x-auto::-webkit-scrollbar {
    display: none;
  }
}
</style>
