<template>
    <aside class="keen-docs-sidebar">
        <brand
            class="keen-docs-sidebar__header"

            :repo-url="repoUrl"
            :version-url="versionUrl"
            :version="version"
        ></brand>

        <div class="keen-docs-sidebar__scrollable">
            <div class="keen-docs-sidebar__description">
                <p>A lightweight collection of essential UI components written with Vue.js and inspired by Material Design.</p>
                <p>Created by <a rel="noopener" target="_blank" href="https://twitter.com/JosephusPaye">Josephus Paye II</a>.</p>
            </div>

            <div class="keen-docs-sidebar__version-select">
                <ui-select
                    :options="['0.8.9', '1.0.0', '1.0.3']"
                    @select="onVersionSelect"
                    v-model="version"
                >Version</ui-select>
            </div>

            <ul class="keen-docs-sidebar__menu">
                <li class="keen-docs-sidebar__menu-section">
                    <div class="keen-docs-sidebar__menu-section-header">Usage</div>

                    <ul class="keen-docs-sidebar__menu-section-links">
                        <li>
                            <a
                                class="keen-docs-sidebar__menu-item"
                                href="https://github.com/JosephusPaye/Keen-UI/tree/master#keen-ui"
                                rel="noopener"
                                target="_blank"
                            >Getting Started <ui-icon>open_in_new</ui-icon></a>
                        </li>

                        <li>
                            <a
                                class="keen-docs-sidebar__menu-item"
                                href="https://github.com/JosephusPaye/Keen-UI/blob/master/Customization.md#customization"
                                rel="noopener"
                                target="_blank"
                            >Customization <ui-icon>open_in_new</ui-icon></a>
                        </li>
                    </ul>
                </li>

                <li class="keen-docs-sidebar__menu-section" v-for="section in menu" :key="section.title">
                    <div class="keen-docs-sidebar__menu-section-header">{{ section.title }}</div>

                    <ul class="keen-docs-sidebar__menu-section-links">
                        <li v-for="item in section.menu" :key="item.path">
                            <router-link
                                class="keen-docs-sidebar__menu-item" exact
                                :to="item.path"
                            >
                                {{ item.title }}
                            </router-link>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
    </aside>
</template>

<script>
import { menu } from './navigation.js';
import Brand from './Brand.vue';
import UiIcon from 'src/UiIcon.vue';
import UiSelect from 'src/UiSelect.vue';

export default {
    components: {
        Brand,
        UiIcon,
        UiSelect
    },

    props: {
        version: String,
        versionUrl: String,
        repoUrl: String
    },

    data() {
        return {
            menu
        };
    },

    methods: {
        onVersionSelect(version) {
            if (version === '1.0.3') {
                return;
            }

            let root = '';
            const component = window.location.hash + (version === '0.8.9' ? '-docs' : '');

            // This is in contrast to localhost, whose root is '/'
            if (window.location.hostname === 'josephuspaye.github.io') {
                root = 'https://josephuspaye.github.io/Keen-UI';
            }

            window.location = `${root}/${version}/${component}`;
        }
    }
};
</script>

<style lang="scss">
@import './styles/imports.scss';

.keen-docs-sidebar {
    background-color: #FFF;
    box-shadow: 0 2px 5px 0 rgba(black, 0.16), 0 2px 10px 0 rgba(black, 0.12);
    display: flex;
    flex-direction: column;
    flex-shrink: 0;
    height: 100vh;
    height: 100vh;
    left: 0;
    position: fixed;
    top: 0;
    width: rem-calc($sidebar-width);
    z-index: 1;

    @media screen and (max-width: $mobile-breakpoint) {
        max-width: rem-calc($sidebar-width-mobile);
        z-index: $z-index-nav-drawer;
    }
}

.keen-docs-sidebar__scrollable {
    height: 100%;
    overflow-y: auto;
    overflow-x: hidden;
}

.keen-docs-sidebar__description {
    color: rgba(black, 0.54);
    padding: rem-calc(16px);

    p {
        margin: 0;

        & + p {
            margin-top: rem-calc(12px);
        }
    }
}

.keen-docs-sidebar__version-select {
    padding: rem-calc(16px);
    padding-top: rem-calc(8px);

    .ui-select {
        margin: 0;
    }

    .ui-select-option {
        min-height: rem-calc(40px);
    }
}

.keen-docs-sidebar__menu,
.keen-docs-sidebar__menu-section-links {
    list-style: none;
    margin: 0;
    padding: 0;
}

.keen-docs-sidebar__menu-section-header {
    color: rgba(black, 0.7);
    font-size: 1.1em;
    font-weight: bold;
    padding: rem-calc(16px);
}

.keen-docs-sidebar__menu-item {
    align-items: center;
    color: rgba(black, 0.87);
    display: flex;
    padding: rem-calc(12px);
    padding-left: rem-calc(40px);

    &.is-active {
        color: $link-color;
        font-weight: 500;
        background-color: rgba(black, 0.05);
    }

    &:hover,
    &:focus {
        text-decoration: none;
        background-color: rgba(black, 0.05);
    }

    .ui-icon {
        font-size: rem-calc(18px);
        margin-left: rem-calc(12px);
        color: #868686;
    }
}

.keen-docs-mobile-sidebar__backdrop {
    background-color: rgba(black, 0.2);
    bottom: 0;
    left: 0;
    position: fixed;
    top: 0;
    width: 100%;
    z-index: $z-index-nav-drawer - 1;
    display: none;

    @media screen and (max-width: $mobile-breakpoint) {
        display: block;
    }
}

.transition-fade-enter-active,
.transition-fade-leave-active {
    transition: opacity 0.2s ease-out;
}

.transition-fade-enter,
.transition-fade-leave-active {
    opacity: 0;
}

.transition-slide-enter-active,
.transition-slide-leave-active {
    transition: transform 0.2s ease-out;
}

.transition-slide-enter,
.transition-slide-leave-active {
    transform: translateX(-$sidebar-width-mobile);
}
</style>
