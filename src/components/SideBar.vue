<template>
    <aside>
        <div class="icons">
            <router-link to="/user" class ="LinkStyle">
                <span @click="sidebarSwitch(0)" class="material-symbols-outlined" :class="{colored: isActive[0]}">
                    account_circle
                </span>
            </router-link>
            <div v-for="icon in iconNames">
                <router-link to="/" class ="LinkStyle">
                    <span @click="sidebarSwitch(icon.pos)" class="material-symbols-outlined" :class="{colored: isActive[icon.pos]}">
                        {{icon.name}}
                    </span>
                </router-link>
            </div>
        </div>
        <div class="bottom-icons">
            <router-link to="/settings" class="LinkStyle">
                <span @click="settingHandler()" class="material-symbols-outlined" :class="{colored: settingsActive}">
                    settings 
                </span>
            </router-link>
        </div>
    </aside>
</template>

<script setup>

    import { ref } from 'vue'
    const iconNames = [
        {
            name: 'search',
            pos: 1              
        },
        {
            name: 'menu',
            pos: 2
        },
        {
            name: 'star',
            pos: 3
        },
        {
            name: 'group',
            pos: 4
        }];

    let isActive = ref([]);
    const settingsActive = ref(false);

    //initializes all icons to be inactive on startup
    for(let i=0; i<=iconNames.length; i++){
        isActive.value.push(false);
    }

    console.log("setup is done!");

    const sidebarSwitch = (index) => {

        //sets all icons to inactive
        for(let i=0; i<=iconNames.length; i++){
            isActive.value[i] = false;
        }

        //only sets selected icon to active
        settingsActive.value = false;
        isActive.value[index] = true;
    }

    const settingHandler = () => {
        //workaround to avoid code duplication, sets first icon to active and then 
        //manually turns it off
        sidebarSwitch(0);
        isActive.value[0]=false;

        //sets settings icon to active
        settingsActive.value = true;

    }

</script>

<style scoped>
    aside {
        display: flex;
        flex-direction: column;
        background-color: var(--dark-main);
        color: var(--light-main);
        width: calc(2rem + 32px);
        min-height: 100vh;
        padding: 1rem;
    }

    .colored{
        color: var(--accent-main);
    }

    .icons{
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        font-size: 2rem;
        padding: 0.5rem 0;
    }

    .material-symbols-outlined{
        font-size: 2rem;
        padding-bottom: 3rem;
    }

    .bottom-icons{
        margin-top: auto;
    }

    .bottom-icons > .material-symbols-outlined{
        padding-bottom: 1.5rem;
    }

    .LinkStyle, .LinkStyle:visited, .LinkStyle:hover, .LinkStyle:active {
        color: inherit;
    }

</style>

