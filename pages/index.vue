<template>
  <div>
    <section class="hero is-info is-bold">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">指標化プロトタイプ</h1>
          <h2 class="subtitle">本番はもっとすごいです！！！</h2>
        </div>
      </div>
    </section>
    <section class="section">
      <div class="container">
        <div v-for="group in questions" :key="group.label">
          <h2 class="title is-4">{{ group.label }}</h2>
          <div class="mb-5" v-for="item in group.children" :key="item.label">
            <h3 class="title is-5">{{ item.label }}</h3>
            <div class="field" v-for="text in item.questions" :key="text">
              <div class="control">
                <label class="checkbox"
                  ><input
                    type="checkbox"
                    v-model="checkedItems"
                    :value="{ group: group.label, item: item.label, text }"
                  />
                  {{ text }}</label
                >
              </div>
            </div>
          </div>
        </div>
        <div>
          <h2 class="title is-4">イノベーション成果指標</h2>
          <div>
            <div class="field">
              <label class="label"
                >プロダクト：これまでに自身が関わった研究成果は社会にどの程度のインパクトを与えましたか</label
              >
              <div class="control">
                <label class="radio"
                  ><input type="radio" value="0" v-model="innovation1" />
                  低い</label
                >
                <label class="radio"
                  ><input type="radio" value="1" v-model="innovation1" />
                  やや低い</label
                >
                <label class="radio"
                  ><input type="radio" value="2" v-model="innovation1" />
                  中間</label
                >
                <label class="radio"
                  ><input type="radio" value="3" v-model="innovation1" />
                  やや高い</label
                >
                <label class="radio"
                  ><input type="radio" value="4" v-model="innovation1" />
                  高い</label
                >
              </div>
            </div>
            <div class="field">
              <label class="label"
                >プロセス：これまでに自身が関わって研究開発システムやそのプロセスをどのくらい改善しましたか</label
              >
              <div class="control">
                <label class="radio"
                  ><input type="radio" value="0" v-model="innovation2" />
                  低い</label
                >
                <label class="radio"
                  ><input type="radio" value="1" v-model="innovation2" />
                  やや低い</label
                >
                <label class="radio"
                  ><input type="radio" value="2" v-model="innovation2" />
                  中間</label
                >
                <label class="radio"
                  ><input type="radio" value="3" v-model="innovation2" />
                  やや高い</label
                >
                <label class="radio"
                  ><input type="radio" value="4" v-model="innovation2" />
                  高い</label
                >
              </div>
            </div>
            <div class="field">
              <label class="label"
                >マーケティング：これまでに自身でアイデアを着想してプロジェクト化をどのくらい実現していますか？</label
              >
              <div class="control">
                <label class="radio"
                  ><input type="radio" value="0" v-model="innovation3" />
                  低い</label
                >
                <label class="radio"
                  ><input type="radio" value="1" v-model="innovation3" />
                  やや低い</label
                >
                <label class="radio"
                  ><input type="radio" value="2" v-model="innovation3" />
                  中間</label
                >
                <label class="radio"
                  ><input type="radio" value="3" v-model="innovation3" />
                  やや高い</label
                >
                <label class="radio"
                  ><input type="radio" value="4" v-model="innovation3" />
                  高い</label
                >
              </div>
            </div>
            <div class="field">
              <label class="label"
                >組織：これまでに自身が関わって所属組織の業務改善や新制度の導入にどのくらい貢献しましたか？</label
              >
              <div class="control">
                <label class="radio"
                  ><input type="radio" value="0" v-model="innovation4" />
                  低い</label
                >
                <label class="radio"
                  ><input type="radio" value="1" v-model="innovation4" />
                  やや低い</label
                >
                <label class="radio"
                  ><input type="radio" value="2" v-model="innovation4" />
                  中間</label
                >
                <label class="radio"
                  ><input type="radio" value="3" v-model="innovation4" />
                  やや高い</label
                >
                <label class="radio"
                  ><input type="radio" value="4" v-model="innovation4" />
                  高い</label
                >
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <section class="section chart">
      <div class="container">
        <rose-chart
          :innerRadius="innerRadius"
          :outerRadius="outerRadius"
          :margin="margin"
          :titleMargin="titleMargin"
          :barOpacity="barOpacity"
          :barAngle="barAngle"
          :barMarginAngle="barMarginAngle"
          :lineColor="lineColor"
          :fontColor="fontColor"
          :fontFamily="fontFamily"
          :tickFontSize="tickFontSize"
          :groupFontSize="groupFontSize"
          :itemFontSize="itemFontSize"
          :titleFontSize="titleFontSize"
          :title="title"
          :values="values"
          :ticks="ticks"
        ></rose-chart>
      </div>
    </section>
    <section class="section">
      <div class="container">
        <div class="buttons is-centered">
          <button class="button is-link" v-on:click="downloadData">
            結果を保存
          </button>
        </div>
      </div>
    </section>
  </div>
</template>
<script>
import RoseChart from "../components/rose-chart.vue";

const download = (url, filename) => {
  const anchor = document.createElement("a");
  anchor.download = filename;
  anchor.href = url;
  anchor.style.display = "none";
  document.body.appendChild(anchor);
  anchor.click();
  document.body.removeChild(anchor);
};

export default {
  data() {
    return {
      innerRadius: 80,
      outerRadius: 250,
      margin: {
        top: 20,
        right: 150,
        bottom: 20,
        left: 150,
      },
      titleMargin: 6,
      barOpacity: "0.8",
      barAngle: 0.28,
      barMarginAngle: 0.05,
      lineColor: "#ccc",
      fontColor: "#555",
      fontFamily: "sans-serif",
      tickFontSize: 10,
      groupFontSize: 10,
      itemFontSize: 8,
      titleFontSize: 14,
      title: ["多様性を", "イノベーションに", "繋ぐ評価"],
      ticks: [0, 20, 40, 60, 80, 100],
      questions: [
        {
          label: "研究に対する態度",
          children: [
            {
              label: "個人",
              questions: [
                "思い描くビジョンの実現や課題解決が実現するまで決して諦めない強い意志を持って取り組んでいる。",
                "思い描くビジョンの実現や課題解決に向けて新しい仕組みを提案している。",
                "他者・異なる考え方から気づきを得て自らのビジョンの実現や課題解決に取り入れている。",
                "個人はビジョンの実現や課題解決に向けて学習・成長に取り組み気づきを得ている。 ",
                "個人のビジョンの実現や課題解決に取り組んでいる。",
                "チームメンバー全体がビジョンや課題を理解して協力し合って解決に取り組んでいる。",
                "個人はビジョンの実現や課題解決に向けて学習・成長に意欲的に取り組んでいる。 ",
                "個人のビジョンや課題を持っている。",
                "社会にとって重要な個人のビジョンや課題を持っている。",
                "ビジョンの実現や課題解決に向けて学習・成長の必要性を感じている。 ",
              ],
            },
            {
              label: "環境",
              questions: [
                "個人のビジョンの実現や課題解決への組織やチームからの支援が活用され記録されている。",
                "個人のビジョンの実現や課題解決への組織やチームからの支援成功例が共有されてる。",
                "個人のビジョンや課題について組織で議論する場がある。",
                "個人のビジョンの実現や課題解決への組織やチームからの支援体制がある。",
                "個人のビジョンや課題と組織のビジョンや課題がマッチしている。",
                "個人のビジョンや課題を組織に発信する場がある。",
              ],
            },
          ],
        },
        {
          label: "ネットワーク・異分野融合",
          children: [
            {
              label: "個人",
              questions: [
                "誠実さ（Integrity）と謙虚さ(Humility)を有している。",
                "異分野の知見を貪欲に取り入れ、垣根無く多様な人材と積極的にコミュニケーションを取っている。",
                "学際的な協働による課題解決アプローチを図っている。",
                "背景（地位や年齢、分野など）に関わりなく、自身も他者もあるがままを尊重し（respect）、敬意を払うことができる。",
                "自分の主たる分野の内にも外にもあらゆることに興味がある。 ",
                "自分の興味関心や考えを大切にしている。",
                "自分自身にも周囲の人間にも思いやり（compassion/consideration）をもって接することができる。 ",
                "他者・異なる考え方から気づきを良く得ている。 ",
              ],
            },
            {
              label: "環境",
              questions: [
                "誰もが必要な情報へのアクセスが可能な透明性・公平性のある仕組みを有している",
                "分野の違う人との出会いや新たな知識に触れる仕掛けを持っている",
                "異分野交流の情報を発信している ",
              ],
            },
          ],
        },
        {
          label: "経験への開かれ",
          children: [
            {
              label: "個人",
              questions: [
                "思い込み（時には偏見）や固定観念から自由であろうと常に心がけている。",
                "自分の心を開きつつ、相手の考えを取り入れることもできる。",
                "自身も含めて誰しも思い込み（時には偏見）や固定観念を持っていると思う。 ",
                "自分は思い込み（時には偏見）や固定観念は持っていないと思う。 ",
              ],
            },
            {
              label: "環境",
              questions: [
                "多様な経験を得た人材が、その経験を活かすことができるように支援や配慮がある。",
                "失敗できる余裕がある ",
                "多様な経験をさせる環境・仕組み、文化風土があり活用されている。 ",
                "多様な（高品質の、好奇心を刺激するような）経験をさせる環境・仕組みを持っている。",
                "失敗が許されないプレッシャーがある",
              ],
            },
          ],
        },
        {
          label: "モチベーション",
          children: [
            {
              label: "環境",
              questions: [
                "自身の興味関心を周囲に広める。",
                "世界や社会からの切望を反映している計画や行動を、仲間と協働し実行している。",
                "自身のモチベーションについて、倫理的な正しさを仲間と議論している。 ",
                "自身の興味関心を深める。",
                "社会を良くしたいと考えた自らの計画や行動は、世界や社会からの切望を反映している。自身のモチベーションについて、倫理的な正しさを意識している。 ",
                "自身の興味関心を育む。",
                "自分が社会を良くするのだと具体的な計画を持って行動する。",
              ],
            },
          ],
        },
        {
          label: "心理的安全性",
          children: [
            {
              label: "環境",
              questions: [
                "チームでは間違いを犯したとしても、責められるようなことはほとんど無い。",
                "チームのメンバーは、問題のある事柄や困難な事案でも言い出すことができる。",
                "チームでは、メンバーが「自分とは違う」ことを理由に他者を拒否することは無い。",
                "チーム内では不安なく思い切った発言や行動ができる。",
                "チームでは他のメンバーに助けを求めることは容易だ。",
                "チームでは誰も私の努力の成果を損なうような行為を故意にしない。",
                "チームのメンバーと一緒に働くことで、私独自の技術や能力が高い評価を受けたり役に立ったりしている。",
              ],
            },
          ],
        },
        {
          label: "リーダーシップ「目利き」",
          children: [
            {
              label: "環境",
              questions: [
                "チームリーダーは組織のビジョンの実現や課題解決に対して10年、20年、30年先を見越している。",
                "チームリーダーは望ましい人材像を明確にしている。",
                "リーダーがメンバーに与えた裁量の経過を評価して対話によるフィードバックをする。",
                "リーダーおよびメンバー全員がリーダーシップを発揮できるようなチームワークを維持するためにリーダーが個を尊重している。",
                "組織のビジョンの実現や課題解決に個人の役割を感じている。",
                "形骸化することなく決定と決定後にも評価に基づいた方針変更をする。",
                "ビジョンの実現のためにあらゆる情報を収集し分析し、先を見通しした目標や解決方法を提示し決断する（先見性）。 ",
                "リーダーがメンバーに一定の裁量を与える。",
                "リーダーが個々のメンバーと対話し、目標の実現あるいは他者の利益に資するようなリーダーシップを個々も発揮できるように働きかけている。",
                "組織のビジョンの実現や課題解決が社会にとって重要で価値があることであると構成員が理解している。",
                "ビジョンの実現のためにあらゆる情報を収集し分析し、目標や解決方法を提示する。",
                "リーダーがメンバーの思いと行動を否定しない。",
                "リーダーはメンバーに業務において客観的に公平である。",
                "リーダーはメンバーと十分なコミュニケーションをとっている。",
                "持続可能性を視野に入れている。",
                "リーダーは思考停止していない。",
                "組織のビジョンや課題を掲げている。",
                "リーダーが目標の実現､あるいは他者の利益に資するようなリーダーシップを発揮している。 ",
                "望ましい人材像が明確になっている（どこにどういうスキルを持った人材が必要か）",
              ],
            },
          ],
        },
        {
          label: "目的と手段の整合",
          children: [
            {
              label: "個人",
              questions: [
                "チームメンバー全体が、ビジョンを理解し共通認識とされている手段を講じながらその手段によって目的（研究課題・技術目標）の達成に向けて協働できる。",
                "ビジョンを達成するための目的と手段が区別され、両者の関係が論理的で明確である。",
                "目的が手段化していないかを定期的に確認し、その項目の現状での妥当性が疑わしければ削除ないしは適宜改変することができる。 ",
                "言語化したビジョンや目的をチームメンバーに共有している。",
                "目的達成を計測する項目が論理的に妥当か定期的に見直している。",
                "目的が手段化していないかを定期的に確認し、その項目の妥当性を説明できチームが納得できる。 ",
                "自身（個人やチーム、組織）の特性を基に、実現可能なビジョンを言語化している。",
                "ビジョンを達成するための中・長期的な目的（研究課題・技術目標）を設定できる。 ",
                "目的が達成できているかを計測することができる。",
                "目的が手段化していないかを確認している。 ",
              ],
            },
            {
              label: "環境",
              questions: [
                "チームメンバー全体が、ビジョンを理解し共通認識とされている手段を講じながらその手段によって目的（研究課題・技術目標）の達成に向けて協働できる。",
                "権限者は目的達成のために最も効果的な権限委譲を行っている。（現場に？） ",
                "言語化したビジョンや目的をチームメンバーに共有している。",
                "目的達成を計測する項目が論理的に妥当か定期的に見直している。",
                "目的が手段化していないかを定期的に確認し、その項目の妥当性を説明できチームが納得できる。",
                "組織はビジョンに即した目的や手段の評価と見直しに有用な肯定的および否定的な情報もチームメンバー（＊もっと良い表現ありますか？）から積極的に収集している。",
                "目的達成のために必要な体制（人材、資金、時間）になっているのかを定期的に評価し見直す。 ",
                "チーム、組織の特性を基に、実現可能なビジョンを言語化している。",
                "ビジョンを達成するための中・長期的な目的（研究課題・技術目標）を設定できる。 ",
                "目的が達成できているかを計測することができる。",
                "目的が手段化していないかを確認し、その項目の妥当性が疑わしければ削除ないしは適宜改変することができる。",
                "目的達成のために必要な体制（人材、資金、時間）を構築する。 ",
              ],
            },
          ],
        },
      ],
      checkedItems: [],
      innovation1: 0,
      innovation2: 0,
      innovation3: 0,
      innovation4: 0,
    };
  },
  computed: {
    values: function () {
      const values = [
        {
          label: "研究に対する態度",
          children: [
            { label: "個人", value: 87, color: "#a6cee3" },
            { label: "環境", value: 81, color: "#1f78b4" },
          ],
        },
        {
          label: "ネットワーク・異分野融合",
          children: [
            { label: "個人", value: 47, color: "#b2df8a" },
            { label: "環境", value: 21, color: "#33a02c" },
          ],
        },
        {
          label: "経験への開かれ",
          children: [
            { label: "個人", value: 57, color: "#fb9a99" },
            { label: "環境", value: 33, color: "#e31a1c" },
          ],
        },
        {
          label: "モチベーション",
          children: [{ label: "環境", value: 67, color: "#6a3d9a" }],
        },
        {
          label: "心理的安全性",
          children: [{ label: "環境", value: 100, color: "#ffff99" }],
        },
        {
          label: "リーダーシップ「目利き」",
          children: [{ label: "環境", value: 63, color: "#b15928" }],
        },
        {
          label: "目的と手段の整合",
          children: [
            { label: "個人", value: 100, color: "#fdbf6f" },
            { label: "環境", value: 100, color: "#ff7f00" },
          ],
        },
      ];
      for (const group of values) {
        for (const item of group.children) {
          const questions = this.questions
            .find(({ label }) => group.label === label)
            .children.find(({ label }) => item.label === label).questions;
          const answers = this.checkedItems.filter(
            (q) => q.group === group.label && q.item === item.label,
          );
          item.value = ((100 * answers.length) / questions.length).toFixed(0);
        }
      }
      return values;
    },
  },
  methods: {
    downloadData: function () {
      let data = "課題,対象,問,値\n";
      for (const group of this.questions) {
        for (const item of group.children) {
          for (const text of item.questions) {
            const checked = this.checkedItems.find(
              (q) =>
                q.group === group.label &&
                q.item === item.label &&
                q.text === text,
            );
            data += `${group.label},${item.label},${text},${checked ? 1 : 0}\n`;
          }
        }
      }
      data += `イノベーション,,プロダクト：これまでに自身が関わった研究成果は社会にどの程度のインパクトを与えましたか,${this.innovation1}\n`;
      data += `イノベーション,,プロセス：これまでに自身が関わって研究開発システムやそのプロセスをどのくらい改善しましたか,${this.innovation2}\n`;
      data += `イノベーション,,マーケティング：これまでに自身でアイデアを着想してプロジェクト化をどのくらい実現していますか？,${this.innovation3}\n`;
      data += `イノベーション,,組織：これまでに自身が関わって所属組織の業務改善や新制度の導入にどのくらい貢献しましたか？,${this.innovation4}\n`;
      const bom = new Uint8Array([0xef, 0xbb, 0xbf]);
      const blob = new Blob([bom, data], { type: "text/csv" });
      download(URL.createObjectURL(blob), "values.csv");
    },
  },
  components: {
    RoseChart,
  },
};
</script>
<style scoped>
.chart {
  background-color: #f7f9fc;
}
</style>
