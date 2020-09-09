<template lang="pug">
.bg
  h1 Sentiment Analysis
  v-row
    v-card.space-all(:height="220", :width="300")
      face-chart(
        :chartdata="chartdataHappy",
        :options="options",
        :height="220",
        :width="300"
      )
    v-card.space-all(:height="220", :width="300")
      face-chart(
        :chartdata="chartdataSad",
        :options="options",
        :height="220",
        :width="300"
      )
    v-card.space-all(:height="220", :width="300")
      face-chart(
        :chartdata="chartdataAngry",
        :options="options",
        :height="220",
        :width="300"
      )
    v-img(:height="250", :width="400", :src="image", contain)
    v-card.space-rigth.space-bottom.space-left(:height="400", :width="240")
      test(
        :chartdata="chartdataConfused",
        :options="options",
        :height="400",
        :width="240"
      )
    v-card.space-rigth.space-bottom.space-left(:height="400", :width="240")
      test(
        :chartdata="chartdataCalm",
        :options="options",
        :height="400",
        :width="240"
      )
    v-card.space-rigth.space-bottom.space-left(:height="400", :width="240")
      test(
        :chartdata="chartdataFear",
        :options="options",
        :height="400",
        :width="240"
      )
    v-card.space-rigth.space-bottom.space-left(:height="400", :width="200")
      graph-doughnut(
        :chartdata="chartdatafirst",
        :options="options",
        :height="200",
        :width="200"
      )
      graph-doughnut(
        :chartdata="chartdatasecond",
        :options="options",
        :height="200",
        :width="200"
      )
    v-img.space-left.space-top(
      :height="250",
      :width="400",
      :src="linedimage",
      contain
    )
  h2.floot-rigth(margin="200") あなたの時給は{{ money }}円です
  h3 {{ gendervalue }} : {{ gender }} Time:{{ time }} Smile:{{ smile }}
</template>
<script>
import FaceChart from "~/components/FaceChart";
import Test from "~/components/Test";
import firebase from "~/plugins/firebase.js";
export default {
  data: function() {
    return {
      data: [],
      wage: 0,
      gender: 0,
      time: 0,
      gendervalue: 0,
      smile: 0,
      image: 0,
      linedimage: 0,
      options: {
        animation: false,
        legend: {
          display: false, //ラベル非表示
        },
      },
    };
  },
  componets: { FaceChart, Test },
  async mounted() {
    await firebase
      .firestore()
      .collection("faces")
      .orderBy("timestamp", "desc")
      .limit(10)
      .onSnapshot((doc) => {
        const chartdata = {
          labels: [],
          datasets: [
            {
              label: "喜び",
              data: [],
            },
          ],
        };
        let labels = [];
        doc.forEach((item) => {
          const d = item.data();
          this.data.push(d);
        });
      });
  },
  methods: {
    async addTestData() {
      const now = new Date();
      await firebase
        .firestore()
        .collection("person")
        .add({
          DateTime: now.getTime(),
          Temp: Math.floor(Math.random() * 100),
        });
    },
  },
  computed: {
    money() {
      return Math.floor(this.wage * 20);
    },
    myStyles() {
      return {
        height: `300px`,
        position: "relative",
      };
    },
    chartdataHappy() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      this.data.forEach((d) => {
        const happy = d.FaceDetails[0].Emotions.find((e) => e.Type === "HAPPY");
        chartdata.datasets[0].data.push(happy.Confidence);
        this.wage = happy.Confidence; //時給計算
        chartdata.labels.push("");
      });
      chartdata.datasets[0].borderColor = "#79f879";
      return chartdata;
    },
    chartdataSad() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      this.data.forEach((d) => {
        const sad = d.FaceDetails[0].Emotions.find((e) => e.Type === "SAD");
        chartdata.datasets[0].data.push(sad.Confidence);
        chartdata.labels.push("");
      });
      chartdata.datasets[0].borderColor = "#79f879";
      return chartdata;
    },
    chartdataAngry() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      this.data.forEach((d) => {
        const angry = d.FaceDetails[0].Emotions.find((e) => e.Type === "ANGRY");
        chartdata.datasets[0].data.push(angry.Confidence);
        chartdata.labels.push("");
      });
      chartdata.datasets[0].borderColor = "#79f879";
      return chartdata;
    },
    chartdataConfused() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      this.data.forEach((d) => {
        const confused = d.FaceDetails[0].Emotions.find(
          (e) => e.Type === "CONFUSED"
        );
        chartdata.datasets[0].data.push(confused.Confidence);
        chartdata.labels.push("");
      });
      chartdata.datasets[0].backgroundColor = "#FF0000";
      return chartdata;
    },
    chartdataCalm() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      this.data.forEach((d) => {
        const calm = d.FaceDetails[0].Emotions.find((e) => e.Type === "CALM");
        chartdata.datasets[0].data.push(calm.Confidence);
        chartdata.labels.push("");
      });
      chartdata.datasets[0].backgroundColor = "#FF0000";
      return chartdata;
    },
    chartdataFear() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      this.data.forEach((d) => {
        const fear = d.FaceDetails[0].Emotions.find((e) => e.Type === "FEAR");
        chartdata.datasets[0].data.push(fear.Confidence);
        chartdata.labels.push("");
      });
      chartdata.datasets[0].backgroundColor = "#FF0000";
      return chartdata;
    },
    chartdatafirst() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      this.data.some((d) => {
        //先頭だけ欲しいのでsome関数をつかった。ただ少し調整が必要かも
        const happy = d.FaceDetails[0].Emotions.find((e) => e.Type === "HAPPY");
        const sad = d.FaceDetails[0].Emotions.find((e) => e.Type === "SAD");
        const angry = d.FaceDetails[0].Emotions.find((e) => e.Type === "ANGRY");
        chartdata.datasets[0].data[0] = happy.Confidence;
        chartdata.datasets[0].data[1] = sad.Confidence;
        chartdata.datasets[0].data[2] = angry.Confidence;
        chartdata.labels.push("");
        return true;
      });
      chartdata.datasets[0].backgroundColor = ["#800000", "#800080", "#ff9e3d"];
      return chartdata;
    },
    chartdatasecond() {
      const chartdata = {};
      chartdata.datasets = [];
      chartdata.datasets[0] = {};
      chartdata.datasets[0].data = [];
      chartdata.labels = [];
      console.log(this.data);
      this.data.some((d) => {
        //先頭だけ欲しいのでsome関数をつかった。ただ少し調整が必要かも
        const confused = d.FaceDetails[0].Emotions.find(
          (e) => e.Type === "CONFUSED"
        );
        const calm = d.FaceDetails[0].Emotions.find((e) => e.Type === "CALM");
        const fear = d.FaceDetails[0].Emotions.find((e) => e.Type === "FEAR");
        this.gender = d.FaceDetails[0].Gender.Confidence;
        this.gendervalue = d.FaceDetails[0].Gender.Value;
        this.time = d.ResponseMetadata.HTTPHeaders.date;
        this.smile = d.FaceDetails[0].Smile.Confidence;
        this.image =
          "https://hacku2020.s3-ap-northeast-1.amazonaws.com/" +
          d.thumbnailImage;
        this.linedimage =
          "https://hacku2020.s3-ap-northeast-1.amazonaws.com/" + d.linedImage;
        chartdata.datasets[0].data[0] = confused.Confidence;
        chartdata.datasets[0].data[1] = calm.Confidence;
        chartdata.datasets[0].data[2] = fear.Confidence;
        chartdata.labels.push("");
        return true;
      });
      chartdata.datasets[0].backgroundColor = ["#FFFF00	", "#0000cd", "#ff00ff"];
      return chartdata;
    },
  },
};
</script>
<style>
.bg {
  background-image: url("AuroraDark.png");
}
.space-all {
  margin: 20px;
}
.space-top {
  margin-top: 20px;
}
.space-right {
  margin-right: 20px;
}
.space-bottom {
  margin-bottom: 20px;
}
.space-left {
  margin-left: 20px;
}
.floot-rigth {
  text-align: right;
}
.padding-top {
  padding-top: 50px;
}
.padding-rigth {
  padding-right: 50px;
}
.padding-bottom {
  padding-bottom: 50px;
}
</style>
