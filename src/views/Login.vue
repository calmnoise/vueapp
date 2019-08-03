<template>
  <v-container fluid fill-height>
    <v-layout align-center justify-center>
      <v-flex xs12 sm8 md4>
        <v-card class="elevation-12">
          <v-toolbar color="primary" dark flat>
            <v-toolbar-title>صفحة تسجيل الدخول</v-toolbar-title>
            <v-spacer></v-spacer>
            <v-tooltip bottom>
              <template v-slot:activator="{ on }">
                <v-btn icon large target="_blank" v-on="on" router to="/forgotpassword">
                  <v-icon>vpn_key</v-icon>
                </v-btn>
              </template>
              <span>نسيت كلمة المرور؟</span>
            </v-tooltip>
            <v-tooltip bottom>
              <template v-slot:activator="{ on }">
                <v-btn icon large target="_blank" v-on="on" router to="/register">
                  <v-icon>person_add</v-icon>
                </v-btn>
              </template>
              <span>ليس لديك حساب؟ أنشئ حساب الآن</span>
            </v-tooltip>
          </v-toolbar>
          <v-card-text>
            <v-form>
              <v-text-field label="اسم المستخدم" name="login" append-icon="person" type="text"></v-text-field>

              <v-text-field
                id="password"
                label="كلمة المرور"
                name="password"
                append-icon="lock"
                type="password"
              ></v-text-field>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn color="primary">تسجيل الدخول</v-btn>
          </v-card-actions>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>
<script>
export default {
  props: {
    source: String
  },
  data: () => ({
    drawer: null
  })
};
</script>