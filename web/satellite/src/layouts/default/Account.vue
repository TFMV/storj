// Copyright (C) 2023 Storj Labs, Inc.
// See LICENSE for copying information.

<template>
    <v-app>
        <session-wrapper>
            <default-bar show-nav-drawer-button />
            <account-nav />
            <default-view />

            <UpgradeAccountDialog v-model="appStore.state.isUpgradeFlowDialogShown" />
            <browser-snackbar-component />
        </session-wrapper>
    </v-app>
</template>

<script setup lang="ts">
import { onBeforeMount } from 'vue';
import { VApp } from 'vuetify/components';

import DefaultBar from './AppBar.vue';
import AccountNav from './AccountNav.vue';
import DefaultView from './View.vue';

import { useAppStore } from '@/store/modules/appStore';
import { useUsersStore } from '@/store/modules/usersStore';
import { useNotify } from '@/composables/useNotify';
import { AnalyticsErrorEventSource } from '@/utils/constants/analyticsEventNames';

import SessionWrapper from '@/components/utils/SessionWrapper.vue';
import UpgradeAccountDialog from '@/components/dialogs/upgradeAccountFlow/UpgradeAccountDialog.vue';
import BrowserSnackbarComponent from '@/components/BrowserSnackbarComponent.vue';

const appStore = useAppStore();
const usersStore = useUsersStore();

const notify = useNotify();

/**
 * Lifecycle hook after initial render.
 * Pre-fetches user's settings.
 */
onBeforeMount(async () => {
    try {
        await usersStore.getSettings();
    } catch (error) {
        notify.notifyError(error, AnalyticsErrorEventSource.ACCOUNT_PAGE);
    }
});
</script>
