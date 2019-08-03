<template>
  <div class="dashboard">
    <h1>الواجهة الرّئيسية</h1>
    <!-- <v-btn small class="hidden-md-and-up">انت تستخدم جوال</v-btn> -->
    <!-- <v-btn class="hidden-md-and-down">للكبار فقط</v-btn> -->

    <v-btn dark fab outlined small color="primary" class="hidden-xs-only">
      <v-icon>email</v-icon>
    </v-btn>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      like_status: false
    };
  },
  methods: {
    addLike: function() {
      this.like_status = !this.like_status;
    }
  }
};
</script>
