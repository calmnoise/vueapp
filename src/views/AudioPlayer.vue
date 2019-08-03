<template>
  <div>
    <!-- <h2>{{get_roundedtime}}</h2> -->
    <v-card class="mt-5 pa-5">
      <v-form>
        <v-form ref="form" v-model="valid" lazy-validation>
          <v-select
            @change="set_announce_type"
            v-model="type_select"
            :items="items"
            :rules="[v => !!v || 'هذا الحقل مطلوب!']"
            label="نوع النّداء"
            :disabled="is_ready"
            required
          ></v-select>

          <v-checkbox
            v-if="announce_type==1"
            v-model="checkbox"
            :rules="[v => !!v || 'يجب عليك تأكيد التّعميد للإرسال!']"
            label="هل تمّ التعميد بإرسال هذا النّداء؟"
            :disabled="is_ready"
            required
          ></v-checkbox>
          <div v-if="announce_type ==1 || announce_type ==2">
            <v-select
              @change="auto_config"
              v-model="mode_select"
              :items="announcementMode"
              :rules="[v => !!v || 'هذا الحقل مطلوب!']"
              label="نمط الإعدادات"
              :disabled="is_ready"
              required
            ></v-select>
            <div v-if="mode_select=='يدوي'">
              <v-select
                v-model="selection_config.timing"
                :items="timing"
                :rules="[v => !!v || 'هذا الحقل مطلوب!']"
                label="وقت الرّحلة"
                :disabled="is_ready"
                required
              ></v-select>
              <v-select
                v-model="selection_config.train"
                :items="trains"
                :rules="[v => !!v || 'هذا الحقل مطلوب!']"
                label="رقم القطار"
                :disabled="is_ready"
                required
              ></v-select>
              <v-select
                @change="stations_select"
                v-model="selection_config.Vstations"
                :items="stations"
                :rules="[v => !!v || 'هذا الحقل مطلوب!']"
                label="محطات الوقوف"
                :disabled="is_ready"
                required
              ></v-select>
            </div>
            <v-select
              v-model="selection_config.platform"
              :items="platform"
              :rules="[v => !!v || 'هذا الحقل مطلوب!']"
              label="رقم الرّصيف"
              :disabled="is_ready"
              required
            ></v-select>
          </div>
          <v-btn color="primary" :disabled="is_ready" @click="setAnnouncement" class="my-3">
            <v-icon left>done_outline</v-icon>
            <span>اعتمد الإعدادات</span>
          </v-btn>
        </v-form>
      </v-form>
    </v-card>
    <v-card dark class="mt-5 pa-3" v-if="is_ready">
      <v-card-title>{{displayed_script_title}}</v-card-title>
      <v-card-text>{{displayed_script}}</v-card-text>

      <vuetify-audio flat class="my-10" :file="audio_file" :ended="audioFinish"></vuetify-audio>
      <v-card-actions>
        <v-btn text color="yellow lighten-3" @click="is_ready=false">
          <v-icon>edit</v-icon>
          <span>تعديل الإعدادات</span>
        </v-btn>
        <v-btn outlined color="red lighten-1" @click="reset">
          <v-icon>add</v-icon>
          <span>ضبط نداء جديد</span>
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>



<script>
import VuetifyAudio from "vuetify-audio";
export default {
  data() {
    return {
      script: {
        c1: " ضيوفنا الكرام، تمّ فتح بوابة الصّعود إلى قطار رقم ",
        c2: " المتّجه إلى مكة المكرمة مرورًا بـ",
        c3: " الرّجاء التوجه إلى رصيف رقم: ",
        c4: " سيكون الصعود من خلال الطابق الثّالث. ",
        c5:
          "نرجو الانتباه، نود تذكير مسافرينا الكرام بالانتباه لأمتعتهم ومتعلقاتهم الشخصية داخل المحطة.",
        c6: "نرجو الانتباه، ممنوع التدخين في أي جزء من المحطة. شكرًا لتعاونكم",
        c7: " نرجو الانتباه، هذا هو النداء الأخير للصعود إلى القطار رقم: ",
        c8: " المتجه إلى مكة المكرمة مرورًا ب",
        c9: " الرّجاء التّوجه إلى الرصيف رقم: "
      },
      script_title: {
        a1: " السكربت الخاص بإعلان النّداء الأول للسّاعة ",
        a2: " السكربت الخاص بإعلان النهائي الأول للسّاعة ",
        belongings: " السكربت الخاص بـنداء الأمتعة ",
        smoking: " السكربت الخاص بنِداء التّدخين "
      },
      is_ready: false,
      tc: {
        key: null,
        timing: null,
        train: null,
        stations: null,
        Vstations: null,
        platform: null
      },
      file: null,
      msgs: ["No message now."],
      valid: true,
      type_select: null,
      mode_select: null,
      selection_config: {
        key: null,
        timing: null,
        train: null,
        Vstations: null,
        stations: null,
        platform: null
      },
      trains: ["0081", "1121"],
      timing: ["6", "8", "12", "15"],
      timingbased: [
        {
          key: "0081",
          timing: "8",
          train: "0081",
          stations: "kj",
          Vstations: "مدينة الملك عبد الله الاقتصادية و جدة"
        },
        {
          key: "1121",
          timing: "15",
          train: "1121",
          stations: "j",
          Vstations: "مدينة جدة"
        }
      ],
      announcementMode: ["تلقائي", "يدوي"],
      stations: ["مدينة جدة", "مدينة الملك عبد الله و جدة"],
      platform: ["1", "2", "3", "4", "5"],
      items: [
        "نداء فتح البوّابات",
        "النّداء الأخير",
        "نداء الأمتعة",
        "نداء التّدخين"
      ],
      checkbox: false,
      announce_type: null
    };
  },
  computed: {
    audio_file: function() {
      return this.announce_type == 1
        ? `./train/${this.tc.train}/${this.tc.stations}/${this.tc.platform}.mp3`
        : this.announce_type == 2
        ? null
        : this.announce_type == 3
        ? `./train/independent/belongings.mp3`
        : this.announce_type == 4
        ? `./train/independent/smoking.mp3`
        : null;
    },
    get_roundedtime: function() {
      const now = new Date();
      return now.getHours();
    },
    displayed_script: function() {
      var scr = this.script;
      var src = this.tc;
      var announ = this.announce_type;

      scr = this.script;
      return announ == 1
        ? scr.c1 +
            src.train +
            scr.c2 +
            src.Vstations +
            scr.c3 +
            src.platform +
            scr.c4
        : announ == 2
        ? scr.c7 + src.train + scr.c8 + src.Vstations + scr.c9 + src.platform
        : announ == 3
        ? scr.c5
        : announ == 4
        ? scr.c6
        : null;
    },
    displayed_script_title: function() {
      var scr = this.script_title;
      var src = this.tc;
      var announ = this.announce_type;
      return announ == 1
        ? scr.a1 + src.timing
        : announ == 2
        ? scr.a2 + src.timing
        : announ == 3
        ? scr.belongings
        : announ == 4
        ? scr.smoking
        : null;
    }
  },

  components: {
    "vuetify-audio": VuetifyAudio
  },
  methods: {
    set_announce_type: function() {
      if (this.type_select == "نداء فتح البوّابات") {
        this.announce_type = 1;
      } else if (this.type_select == "النّداء الأخير") {
        this.announce_type = 2;
      } else if (this.type_select == "نداء الأمتعة") {
        this.announce_type = 3;
      } else if (this.type_select == "نداء التّدخين") {
        this.announce_type = 4;
      } else {
        this.announce_type = null;
      }
    },
    audioFinish() {
      this.msgs.push("The audio finished.");
      this.tc = {
        key: null,
        timing: null,
        train: null,
        stations: null,
        platform: null
      };
      this.$refs.form.reset();
      this.is_ready = false;
    },
    validate() {
      if (this.$refs.form.validate()) {
        this.snackbar = true;
      }
    },
    reset() {
      this.$refs.form.reset();
      this.tc = {
        key: null,
        timing: null,
        train: null,
        stations: null,
        platform: null
      };
      this.is_ready = false;
    },
    resetValidation() {
      this.$refs.form.resetValidation();
    },
    stations_select() {
      if (this.selection_config.Vstations == "مدينة الملك عبد الله و جدة") {
        this.selection_config.stations = "kj";
        console.log("تم تعيين الوجهة إلى مدينة الملك عبد الله وجدة ثم مكة");
      } else {
        this.selection_config.stations = "j";
        console.log("تم تعيين الوجهة إلى مكة مرورًا بجدة فقط!");
      }
    },
    auto_config: function() {
      if (this.mode_select == "تلقائي") {
        var i;
        for (i = 0; i < this.timingbased.length; i++) {
          if (
            this.get_roundedtime >= this.timingbased[i].timing - 1 &&
            this.get_roundedtime <= this.timingbased[i].timing + 1
          ) {
            this.selection_config.timing = this.timingbased[i].timing;
            this.selection_config.train = this.timingbased[i].train;
            this.selection_config.stations = this.timingbased[i].stations;
            this.selection_config.Vstations = this.timingbased[i].Vstations;
            console.log(
              "تم الضّبط التّلقائي بنجاح على إعدادات رحلة السّاعة " +
                this.timingbased[i].timing
            );
            break;
          } else if (i == this.timingbased.length - 1) {
            console.log("لا توجد رحلة قريبة! ");
          }
        }
      } else {
        console.log("تم التّعيين كيدوي ");
      }
    },
    setAnnouncement: function() {
      if (this.$refs.form.validate()) {
        this.tc = this.selection_config;
        console.log("تم ضبط الإعدادات بنجاح");
        this.is_ready = true;
      } else {
        console.log("تأكد من ملأ المعلومات بشكل صحيح!");
      }
    }
  }
};
</script>