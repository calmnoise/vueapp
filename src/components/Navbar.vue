<template>
  <div>
    <v-navigation-drawer app hide-overlay right dark expand-on-hover v-model="drawer">
      <v-list nav rounded>
        <v-list-item two-line>
          <v-list-item-avatar>
            <img
              class="ml-2"
              src="https://mediaaws.almasryalyoum.com/news/large/2017/07/18/687455_0.PNG"
            />
          </v-list-item-avatar>

          <v-list-item-content>
            <v-list-item-title>قطار الحرمين السريع</v-list-item-title>
            <v-list-item-subtitle>المدينة المنورة</v-list-item-subtitle>
          </v-list-item-content>
        </v-list-item>

        <v-divider class="my-3"></v-divider>
        <v-list-item color="white" v-for="item in links" :key="item.text" router :to="item.route">
          <v-list-item-icon>
            <v-icon class="mr-2" right>{{ item.icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content>
            <v-list-item-title>{{ item.text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>
    <v-app-bar app flat class="grey lighten-2">
      <v-toolbar-title class="grey--text">
        <span class="font-weight-light">نظام</span>
        <span>النداء</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn key="logout" text color="grey" router to="/audio">
        <span>نظام النّداء</span>
        <v-icon right id="forcertl">surround_sound</v-icon>
      </v-btn>
    </v-app-bar>
  </div>
</template>
<script>
export default {
  data() {
    return {
      drawer: true,
      links: [
        { text: "الرئيسية", icon: "dashboard", route: "/" },
        { text: "نظام النّداء", icon: "surround_sound", route: "/audio" },
        { text: "جدول الرحلات", icon: "calendar_today", route: "/team" }
      ],
      loginstatus: false
    };
  },
  methods: {
    nothing() {
      //nothing
    }
  }
};
</script>