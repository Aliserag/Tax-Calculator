<template>
  <div>
    <h1>Esti The Tax Bot</h1>
    <div class="assumptionTableDiv">
      <table>
        <tr>
          <th colspan="2">Your Income Assumptions</th>
        </tr>
        <tr>
          <td width="60%">Employment Income</td>
          <td width="40%">
            <span> $ </span
            ><input
              type="number"
              id="employmentIncome"
              class="userInput1 input-number–noSpinners"
              value="55000"
              min="0"
              max="10000000"
            />
          </td>
        </tr>

        <tr>
          <td>Capital Gains</td>
          <td>
            <span> $ </span
            ><input
              type="number"
              id="capitalGains"
              class="userInput1 input-number–noSpinners"
              value="0"
              min="0"
              max="10000000"
            />
          </td>
        </tr>

        <tr>
          <td>Eligible Canadian Dividends</td>
          <td>
            <span> $ </span
            ><input
              type="number"
              id="eligibleDividends"
              class="userInput1 input-number–noSpinners"
              value="0"
              min="0"
              max="10000000"
            />
          </td>
        </tr>

        <tr>
          <td>Non-Eligible Dividends</td>
          <td>
            <span> $ </span
            ><input
              type="number"
              id="nonEligibleDividends"
              class="userInput1 input-number–noSpinners"
              value="0"
              min="0"
              max="10000000"
            />
          </td>
        </tr>

        <tr>
          <td>Other Income</td>
          <td>
            <span> $ </span
            ><input
              type="number"
              id="otherIncome"
              class="userInput1 input-number–noSpinners"
              value="0"
              min="0"
              max="10000000"
            />
          </td>
        </tr>

        <tr id="totalPreTaxIncomeRow">
          <td class="totalPreTaxIncomeData">Total Pre-Tax Income</td>
          {{
            this.totalPreTaxIncomeOutput
          }}
          <td class="totalPreTaxIncomeData" id="totalPreTaxIncomeOutput"></td>
        </tr>
      </table>

      <table>
        <tr>
          <th colspan="2">Your RRSP Assumption</th>
        </tr>
        <tr>
          <td width="60%">RRSP Contribution</td>
          <td width="40%">
            <span> $ </span
            ><input
              type="number"
              id="RRSPContribution"
              class="userInput1 input-number–noSpinners"
              value="0"
              min="0"
              max="10000000"
            />
          </td>
        </tr>
      </table>
    </div>

    <div id="mainTableDiv">
      <table id="mainTable">
        <th id="header1">Province / Territory</th>
        <th id="header2">Total Pre-Tax Income</th>
        <th id="header3">Federal Tax</th>
        <th id="header4">Provincial Tax</th>
        <th id="header5">CPP / QPP</th>
        <th id="header6">EI</th>
        <th id="header7">QPIP</th>
        <th id="header8">Total Taxes</th>
        <th id="header9">After-Tax Income</th>
        <th id="header10">Average Tax Rate</th>
        <th id="header11">Marginal Tax Rate</th>
      </table>
    </div>

    <!-- <tr>
      <td class="analysis3Assumption">Province Selected</td>
      <td class="analysis3Assumption">
        <select name="regionA" class="userInput3" id="regionDropDownListA">
          <option value="AB" selected="selected">ON</option>
          <option value="BC">BC</option>
          <option value="MB">MB</option>
          <option value="NB">NB</option>
          <option value="NL">NL</option>
          <option value="NS">NS</option>
          <option value="NT">NT</option>
          <option value="NU">NU</option>
          <option value="ON">ON</option>
          <option value="PE">PE</option>
          <option value="QC">QC</option>
          <option value="SK">SK</option>
          <option value="YT">YT</option>
        </select>
      </td>
    </tr> -->

    <div class="text-center m-5">
      <b-form-select class="w-50" v-model="selected">
        <option value="AB">AB</option>
        <option value="BC">BC</option>
        <option value="MB">MB</option>
        <option value="NB">NB</option>
        <option value="NL">NL</option>
        <option value="NS">NS</option>
        <option value="NT">NT</option>
        <option value="NU">NU</option>
        <option value="ON">ON</option>
        <option value="PE">PE</option>
        <option value="QC">QC</option>
        <option value="SK">SK</option>
        <option value="YT">YT</option>
      </b-form-select>
    </div>

    <h1>Index of Province</h1>
    {{ provinceIndex }}
    <h1>Tax info for selected</h1>
    Federal Tax: ${{ federalTax }}

    <!-- <div class="assumptionTableDiv">
      <table>
        <tr>
          <td width="60%" id="analysis2Label">Annual After-Tax Income</td>
          <td width="40%" id="analysis2Assumption">
            <span> $ </span
            ><input
              type="number"
              id="afterTaxIncomeAssumption"
              class="userInput2 input-number–noSpinners"
              value="40000"
              min="0"
              max="10000000"
              step="1000"
            />
          </td>
        </tr>
      </table>
    </div> -->

    <!-- <div class="assumptionTableDiv">
      <table>
        <tr>
          <td class="analysis3Assumption">Province / Territory #1</td>
          <td class="analysis3Assumption">
            <select name="regionA" class="userInput3" id="regionDropDownListA">
              <option value="AB" selected="selected">AB</option>
              <option value="BC">BC</option>
              <option value="MB">MB</option>
              <option value="NB">NB</option>
              <option value="NL">NL</option>
              <option value="NS">NS</option>
              <option value="NT">NT</option>
              <option value="NU">NU</option>
              <option value="ON">ON</option>
              <option value="PE">PE</option>
              <option value="QC">QC</option>
              <option value="SK">SK</option>
              <option value="YT">YT</option>
            </select>
          </td>
        </tr>

        <tr>
          <td class="analysis3Assumption">Province / Territory #2</td>
          <td class="analysis3Assumption">
            <select name="regionB" class="userInput3" id="regionDropDownListB">
              <option value="AB">AB</option>
              <option value="BC">BC</option>
              <option value="MB">MB</option>
              <option value="NB">NB</option>
              <option value="NL">NL</option>
              <option value="NS">NS</option>
              <option value="NT">NT</option>
              <option value="NU">NU</option>
              <option value="ON" selected="selected">ON</option>
              <option value="PE">PE</option>
              <option value="QC">QC</option>
              <option value="SK">SK</option>
              <option value="YT">YT</option>
            </select>
          </td>
        </tr>

      </table>
    </div> -->
  </div>
</template>

<script>
// import Charts from "chart.js";

export default {
  components: {
    // HollowDotsSpinner,
    // Chart: Charts,
  },
  name: "HelloWorld",
  data() {
    return {
      employmentIncome: 0,
      capitalGains: 0,
      eligibleDividends: 0,
      nonEligibleDividends: 0,
      otherIncome: 0,
      totalGrossIncome: 0,
      RRSPContribution: 0,
      capitalGainsRate: 0.5,
      eligibleGrossUp: 0.38,
      nonEligibleGrossUp: 0.15,
      totalTaxableIncome: 0,

      labelArray: [
        "AB",
        "BC",
        "MB",
        "NB",
        "NL",
        "NT",
        "NS",
        "NU",
        "ON",
        "PE",
        "QC",
        "SK",
        "YT",
      ],

      chart: {},
      afterTaxIncomeBarChart: document.getElementById("afterTaxIncomeBarChart"),

      chart2: {},
      totalTaxBarChart: document.getElementById("totalTaxBarChart"),

      chart3: {},
      avgTaxRateBarChart: document.getElementById("avgTaxRateBarChart"),

      graphColourArray: [
        "#F8F41B",
        "#EDE02B",
        "#E3CD3C",
        "#D9BA4D",
        "#CFA65E",
        "#C4936E",
        "#BA807F",
        "#B06C90",
        "#A659A1",
        "#9B46B1",
        "#9132C2",
        "#871FD3",
        "#7D0CE4",
      ],
      graphColourArray2: [
        "#084081",
        "#125F8B",
        "#1F7F95",
        "#2D9E9F",
        "#3CAA97",
        "#4EB490",
        "#60BE8D",
        "#74C88D",
        "#8AD393",
        "#A4DDA1",
        "#C4E7BA",
        "#E0F1D4",
        "#F7FCF0",
      ],

      federalTaxROC: 0,
      federalTaxQC: 0,
      ABTax: 0,
      BCTax: 0,
      MBTax: 0,
      NBTax: 0,
      NLTax: 0,
      NTTax: 0,
      NSTax: 0,
      NUTax: 0,
      ONTax: 0,
      PETax: 0,
      QCTax: 0,
      SKTax: 0,
      YTTax: 0,

      totalABTaxes: 0,
      totalBCTaxes: 0,
      totalMBTaxes: 0,
      totalNBTaxes: 0,
      totalNLTaxes: 0,
      totalNTTaxes: 0,
      totalNSTaxes: 0,
      totalNUTaxes: 0,
      totalONTaxes: 0,
      totalPETaxes: 0,
      totalQCTaxes: 0,
      totalSKTaxes: 0,
      totalYTTaxes: 0,

      totalGrossIncomeArray: [],
      federalTaxArray: [],
      provincialTaxArray: [],
      CPPTaxArray: [],
      EITaxArray: [],
      QPPTaxArray: [],
      CPPQPPTaxArray: [],
      QPIPTaxArray: [],
      totalTaxesArray: [],
      netIncomeArray: [],
      avgTaxRateArray: [],
      marginalTaxRateArray: [],

      CPPTax: 0,
      enhancedCPPTax: 0,
      baseCPPTax: 0,

      QPPTax: 0,
      enhancedQPPTax: 0,
      baseQPPTax: 0,

      EITaxROC: 0,
      EITaxQC: 0,
      QPIPTax: 0,

      OHIPTax: 0,

      CPPExemption: 3500,
      CPPContributionRate: 0.0525,
      CPPMaxContribution: 2898,
      enhancedCPPRate: 0.003,

      QPPExemption: 3500,
      QPPContributionRate: 0.057,
      QPPMaxContribution: 3146.4,
      enhancedQPPRate: 0.003,

      EIROCRate: 0.0158,
      EIROCMax: 856.36,

      EIQCRate: 0.012,
      EIQCMax: 650.4,

      QPIPExemption: 2000,
      QPIPContributionRate: 0.00494,
      QPIPMaxContribution: 387.79,
      // QPIPTax: 0,

      OHIPContribution: 0,

      //Federal assumptions
      federalBracket1: 48535,
      federalBracket2: 97069,
      federalBracket3: 150473,
      federalBracket4: 214368,
      federalRate1: 0.15,
      federalRate2: 0.205,
      federalRate3: 0.26,
      federalRate4: 0.29,
      federalRate5: 0.33,

      baseFederalPersonalAmount: 12298,
      maxEnhancedBPA: 931,
      enhancedBPA: 0,
      totalFederalPersonalAmount: 0,
      enhancedBPAThresholdStart: 150473,
      enhancedBPAThresholdEnd: 214368,

      federalEligibleDivCredit: 0.1502,
      federalNonEligibleDivCredit: 0.090301,
      federalTaxCreditRate: 0.15,
      federalEmploymentAmount: 0,
      federalEmploymentMax: 1245,

      QCAbatementRate: 0.165,

      //AB assumptions
      ABBracket1: 131220,
      ABBracket2: 157464,
      ABBracket3: 209952,
      ABBracket4: 314928,
      ABRate1: 0.1,
      ABRate2: 0.12,
      ABRate3: 0.13,
      ABRate4: 0.14,
      ABRate5: 0.15,
      ABPersonalAmount: 19369,
      ABEligibleDivCredit: 0.1,
      ABNonEligibleDivCredit: 0.0218,
      ABTaxCreditRate: 0.1,

      //BC assumptions
      BCBracket1: 41725,
      BCBracket2: 83451,
      BCBracket3: 95812,
      BCBracket4: 116344,
      BCBracket5: 157748,
      BCBracket6: 220000,
      BCRate1: 0.0506,
      BCRate2: 0.077,
      BCRate3: 0.105,
      BCRate4: 0.1229,
      BCRate5: 0.147,
      BCRate6: 0.168,
      BCRate7: 0.205,
      BCPersonalAmount: 10949,
      BCEligibleDivCredit: 0.12,
      BCNonEligibleDivCredit: 0.0196,
      BCTaxCreditRate: 0.0506,

      //MB assumptions
      MBBracket1: 33389,
      MBBracket2: 72164,
      MBRate1: 0.108,
      MBRate2: 0.1275,
      MBRate3: 0.174,
      MBPersonalAmount: 9838,
      MBEligibleDivCredit: 0.08,
      MBNonEligibleDivCredit: 0.007835,
      MBTaxCreditRate: 0.108,

      //NB assumptions
      NBBracket1: 43401,
      NBBracket2: 86803,
      NBBracket3: 141122,
      NBBracket4: 160776,
      NBRate1: 0.0968,
      NBRate2: 0.1482,
      NBRate3: 0.1652,
      NBRate4: 0.1784,
      NBRate5: 0.203,
      NBPersonalAmount: 10459,
      NBEligibleDivCredit: 0.14,
      NBNonEligibleDivCredit: 0.0275,
      NBTaxCreditRate: 0.0968,

      //NL assumptions
      NLBracket1: 37929,
      NLBracket2: 75858,
      NLBracket3: 135432,
      NLBracket4: 189604,
      NLRate1: 0.087,
      NLRate2: 0.145,
      NLRate3: 0.158,
      NLRate4: 0.173,
      NLRate5: 0.183,
      NLPersonalAmount: 9498,
      NLEligibleDivCredit: 0.054,
      NLNonEligibleDivCredit: 0.035,
      NLTaxCreditRate: 0.087,

      //NT assumptions
      NTBracket1: 43957,
      NTBracket2: 87916,
      NTBracket3: 142932,
      NTRate1: 0.059,
      NTRate2: 0.086,
      NTRate3: 0.122,
      NTRate4: 0.1405,
      NTPersonalAmount: 15093,
      NTEligibleDivCredit: 0.115,
      NTNonEligibleDivCredit: 0.06,
      NTTaxCreditRate: 0.059,

      //NS assumptions
      NSBracket1: 29590,
      NSBracket2: 59180,
      NSBracket3: 93000,
      NSBracket4: 150000,
      NSRate1: 0.0879,
      NSRate2: 0.1495,
      NSRate3: 0.1667,
      NSRate4: 0.175,
      NSRate5: 0.21,
      NSPersonalAmount: 8481,
      NSEnhancedPersonalAmount: 0,
      NSTotalPersonalAmount: 0,
      NSEligibleDivCredit: 0.0885,
      NSNonEligibleDivCredit: 0.0299,
      NSTaxCreditRate: 0.0879,

      //NU assumptions
      NUBracket1: 46277,
      NUBracket2: 92555,
      NUBracket3: 150473,
      NURate1: 0.04,
      NURate2: 0.07,
      NURate3: 0.09,
      NURate4: 0.115,
      NUPersonalAmount: 16304,
      NUEligibleDivCredit: 0.0551,
      NUNonEligibleDivCredit: 0.0261,
      NUTaxCreditRate: 0.04,

      //ON assumptions
      ONBracket1: 44740,
      ONBracket2: 89482,
      ONBracket3: 150000,
      ONBracket4: 220000,
      ONRate1: 0.0505,
      ONRate2: 0.0915,
      ONRate3: 0.1116,
      ONRate4: 0.1216,
      ONRate5: 0.1316,
      ONPersonalAmount: 10783,
      ONEligibleDivCredit: 0.1,
      ONNonEligibleDivCredit: 0.029863,
      ONTaxCreditRate: 0.0505,

      ONSurtax1: 4830,
      ONSurtax2: 6182,
      ONSurtax1Rate: 0.2,
      ONSurtax2Rate: 0.36,

      //PE assumptions
      PEBracket1: 31984,
      PEBracket2: 63969,
      PERate1: 0.098,
      PERate2: 0.138,
      PERate3: 0.167,
      PEPersonalAmount: 10000,
      PEEligibleDivCredit: 0.105,
      PENonEligibleDivCredit: 0.0274,
      PETaxCreditRate: 0.098,

      PESurtax1: 12500,
      PESurtax1Rate: 0.1,

      //QC assumptions
      QCBracket1: 44545,
      QCBracket2: 89080,
      QCBracket3: 108390,
      QCRate1: 0.15,
      QCRate2: 0.2,
      QCRate3: 0.24,
      QCRate4: 0.2575,
      QCPersonalAmount: 15532,
      QCEligibleDivCredit: 0.117,
      QCNonEligibleDivCredit: 0.0477,
      QCTaxCreditRate: 0.15,

      //SK assumptions
      SKBracket1: 45225,
      SKBracket2: 129214,
      SKRate1: 0.105,
      SKRate2: 0.125,
      SKRate3: 0.145,
      SKPersonalAmount: 16065,
      SKEligibleDivCredit: 0.11,
      SKNonEligibleDivCredit: 0.03362,
      SKTaxCreditRate: 0.105,

      //YT assumptions
      YTBracket1: 48535,
      YTBracket2: 97069,
      YTBracket3: 150473,
      YTBracket4: 500000,
      YTRate1: 0.064,
      YTRate2: 0.09,
      YTRate3: 0.109,
      YTRate4: 0.128,
      YTRate5: 0.15,
      YTPersonalAmount: 12298,
      YTEligibleDivCredit: 0.1202,
      YTNonEligibleDivCredit: 0.023,
      YTTaxCreditRate: 0.064,

      //Analysis #1
      // totalPreTaxIncomeOutput: document.getElementById(
      //   "totalPreTaxIncomeOutput"
      // ),
      totalPreTaxIncomeOutput: 0,
      //Income Description
      moreInfoSwitch: 0,
      moreInfoButton: document.getElementById("moreInfoButton"),
      incomeDescriptionDiv: document.getElementById("incomeDescriptionDiv"),

      //Analysis #2 Assumptions
      netIncomeGoal: 0,
      grossIncomeArray: [],

      chart4: {},
      grossIncomeBarChart: document.getElementById("grossIncomeBarChart"),

      //Analysis #3 Assumptions

      provinceA: "AB",
      provinceB: "BC",

      analysis3Para: document.getElementById("analysis3Para"),

      grossIncomeLabelArray: [],
      provinceANetIncomeArray: [],
      provinceBNetIncomeArray: [],
      provinceDeltaArray: [],

      //Fill Table function
      mainTable: document.getElementById("mainTable"),
      dataLength: 13,
      numberCol: 11,

      labelArraySorted: [],
      totalGrossIncomeArraySorted: [],
      federalTaxArraySorted: [],
      provincialTaxArraySorted: [],
      CPPTaxArraySorted: [],
      EITaxArraySorted: [],
      QPPTaxArraySorted: [],
      CPPQPPTaxArraySorted: [],
      QPIPTaxArraySorted: [],
      totalTaxesArraySorted: [],
      netIncomeArraySorted: [],
      avgTaxRateArraySorted: [],
      marginalTaxRateArraySorted: [],

      selected: "ON",
    };
  },
  props: {
    msg: String,
  },
  mounted() {
    this.addInputEventListeners();
    this.getUserInputs();

    this.calculateTaxes();
    this.showOutputs();
    this.fillTable();

    // this.seekPreTaxIncome(this.netIncomeGoal);
    // this.showOutputs2();

    // this.provinceComparison(this.provinceA, this.provinceB);
    // this.showOutputs3();
  },
  computed: {
    provinceIndex() {
      return this.labelArraySorted.indexOf(this.selected);
    },
    // totalGross() {},
    federalTax() {
      return Math.round(
        this.federalTaxArraySorted[this.labelArraySorted.indexOf(this.selected)]
      ).toLocaleString();
    },
  },
  methods: {
    addInputEventListeners() {
      var inputsArray1 = document.getElementsByClassName("userInput1");
      var inputsArray2 = document.getElementsByClassName("userInput2");
      var inputsArray3 = document.getElementsByClassName("userInput3");

      for (var i = 0; i < inputsArray1.length; i++) {
        inputsArray1[i].addEventListener(
          "change",
          this.refreshAnalysis1,
          false
        );
      }

      for (i = 0; i < inputsArray2.length; i++) {
        inputsArray2[i].addEventListener(
          "change",
          this.refreshAnalysis2,
          false
        );
      }

      for (i = 0; i < inputsArray3.length; i++) {
        inputsArray3[i].addEventListener(
          "change",
          this.refreshAnalysis3,
          false
        );
      }
    },

    getUserInputs() {
      //Analysis #1
      this.employmentIncome = Number(
        document.getElementById("employmentIncome").value
      );
      this.capitalGains = Number(document.getElementById("capitalGains").value);
      this.eligibleDividends = Number(
        document.getElementById("eligibleDividends").value
      );
      this.nonEligibleDividends = Number(
        document.getElementById("nonEligibleDividends").value
      );
      this.otherIncome = Number(document.getElementById("otherIncome").value);
      this.RRSPContribution = Number(
        document.getElementById("RRSPContribution").value
      );

      this.totalPreTaxIncomeOutput =
        this.employmentIncome +
        this.capitalGains +
        this.eligibleDividends +
        this.nonEligibleDividends +
        this.otherIncome;
      // this.totalPreTaxIncomeOutput.innerHTML =
      //   "$" +
      //   (
      //     this.employmentIncome +
      //     this.capitalGains +
      //     this.eligibleDividends +
      //     this.nonEligibleDividends +
      //     this.otherIncome
      //   ).toLocaleString();

      //Need to calculate CPP first before taxable income, given that enhanced CPP contribution is netted off of total taxable income
      this.calculateCPP();

      this.totalTaxableIncome =
        this.employmentIncome +
        this.capitalGains * this.capitalGainsRate +
        this.eligibleDividends * (1 + this.eligibleGrossUp) +
        this.nonEligibleDividends * (1 + this.nonEligibleGrossUp) +
        this.otherIncome -
        this.RRSPContribution -
        this.enhancedCPPTax;

      this.federalEmploymentAmount = Math.min(
        this.federalEmploymentMax,
        this.employmentIncome
      );

      //Analysis #2
      // this.netIncomeGoal = Number(
      //   document.getElementById("afterTaxIncomeAssumption").value
      // );

      //Analysis #3
      // this.provinceA = String(
      //   document.getElementById("regionDropDownListA").value
      // );
      // this.provinceB = String(
      //   document.getElementById("regionDropDownListB").value
      // );
    },

    calculateTaxes() {
      this.calculateCPP();
      this.calculateQPP();
      this.calculateEI();
      this.calculateQPIP();
      this.calculateOHIP();

      this.calculateFedTaxes();

      this.calculateAB();
      this.calculateBC();
      this.calculateMB();
      this.calculateNB();
      this.calculateNL();
      this.calculateNT();
      this.calculateNS();
      this.calculateNU();
      this.calculateON();
      this.calculatePE();
      this.calculateQC();
      this.calculateSK();
      this.calculateYT();

      //calculate total taxes
      this.totalABTaxes =
        this.federalTaxROC + this.ABTax + this.CPPTax + this.EITaxROC;

      this.totalBCTaxes =
        this.federalTaxROC + this.BCTax + this.CPPTax + this.EITaxROC;
      this.totalMBTaxes =
        this.federalTaxROC + this.MBTax + this.CPPTax + this.EITaxROC;
      this.totalNBTaxes =
        this.federalTaxROC + this.NBTax + this.CPPTax + this.EITaxROC;
      this.totalNLTaxes =
        this.federalTaxROC + this.NLTax + this.CPPTax + this.EITaxROC;
      this.totalNTTaxes =
        this.federalTaxROC + this.NTTax + this.CPPTax + this.EITaxROC;
      this.totalNSTaxes =
        this.federalTaxROC + this.NSTax + this.CPPTax + this.EITaxROC;
      this.totalNUTaxes =
        this.federalTaxROC + this.NUTax + this.CPPTax + this.EITaxROC;
      this.totalONTaxes =
        this.federalTaxROC + this.ONTax + this.CPPTax + this.EITaxROC;
      this.totalPETaxes =
        this.federalTaxROC + this.PETax + this.CPPTax + this.EITaxROC;
      this.totalQCTaxes =
        this.federalTaxQC +
        this.QCTax +
        this.QPPTax +
        this.EITaxQC +
        this.QPIPTax;

      this.totalSKTaxes =
        this.federalTaxROC + this.SKTax + this.CPPTax + this.EITaxROC;
      this.totalYTTaxes =
        this.federalTaxROC + this.YTTax + this.CPPTax + this.EITaxROC;
    },

    showOutputs() {
      this.totalGrossIncome =
        this.employmentIncome +
        this.capitalGains +
        this.eligibleDividends +
        this.nonEligibleDividends +
        this.otherIncome;

      var netIncomeAB = this.totalGrossIncome - this.totalABTaxes;
      var netIncomeBC = this.totalGrossIncome - this.totalBCTaxes;
      var netIncomeMB = this.totalGrossIncome - this.totalMBTaxes;
      var netIncomeNB = this.totalGrossIncome - this.totalNBTaxes;
      var netIncomeNL = this.totalGrossIncome - this.totalNLTaxes;
      var netIncomeNT = this.totalGrossIncome - this.totalNTTaxes;
      var netIncomeNS = this.totalGrossIncome - this.totalNSTaxes;
      var netIncomeNU = this.totalGrossIncome - this.totalNUTaxes;
      var netIncomeON = this.totalGrossIncome - this.totalONTaxes;
      var netIncomePE = this.totalGrossIncome - this.totalPETaxes;
      var netIncomeQC = this.totalGrossIncome - this.totalQCTaxes;
      var netIncomeSK = this.totalGrossIncome - this.totalSKTaxes;
      var netIncomeYT = this.totalGrossIncome - this.totalYTTaxes;

      var ABAvgRate = this.totalABTaxes / this.totalGrossIncome;
      var BCAvgRate = this.totalBCTaxes / this.totalGrossIncome;
      var MBAvgRate = this.totalMBTaxes / this.totalGrossIncome;
      var NBAvgRate = this.totalNBTaxes / this.totalGrossIncome;
      var NLAvgRate = this.totalNLTaxes / this.totalGrossIncome;
      var NTAvgRate = this.totalNTTaxes / this.totalGrossIncome;
      var NSAvgRate = this.totalNSTaxes / this.totalGrossIncome;
      var NUAvgRate = this.totalNUTaxes / this.totalGrossIncome;
      var ONAvgRate = this.totalONTaxes / this.totalGrossIncome;
      var PEAvgRate = this.totalPETaxes / this.totalGrossIncome;
      var QCAvgRate = this.totalQCTaxes / this.totalGrossIncome;
      var SKAvgRate = this.totalSKTaxes / this.totalGrossIncome;
      var YTAvgRate = this.totalYTTaxes / this.totalGrossIncome;

      this.totalGrossIncomeArray = [
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
        this.totalGrossIncome,
      ];

      this.federalTaxArray = [
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxROC,
        this.federalTaxQC,
        this.federalTaxROC,
        this.federalTaxROC,
      ];
      this.provincialTaxArray = [
        this.ABTax,
        this.BCTax,
        this.MBTax,
        this.NBTax,
        this.NLTax,
        this.NTTax,
        this.NSTax,
        this.NUTax,
        this.ONTax,
        this.PETax,
        this.QCTax,
        this.SKTax,
        this.YTTax,
      ];
      this.CPPTaxArray = [
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        "n/a",
        this.CPPTax,
        this.CPPTax,
      ];
      this.EITaxArray = [
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxROC,
        this.EITaxQC,
        this.EITaxROC,
        this.EITaxROC,
      ];
      this.QPPTaxArray = [
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        this.QPPTax,
        "n/a",
        "n/a",
      ];
      this.CPPQPPTaxArray = [
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.CPPTax,
        this.QPPTax,
        this.CPPTax,
        this.CPPTax,
      ];
      this.QPIPTaxArray = [
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        "n/a",
        this.QPIPTax,
        "n/a",
        "n/a",
      ];
      this.totalTaxesArray = [
        this.totalABTaxes,
        this.totalBCTaxes,
        this.totalMBTaxes,
        this.totalNBTaxes,
        this.totalNLTaxes,
        this.totalNTTaxes,
        this.totalNSTaxes,
        this.totalNUTaxes,
        this.totalONTaxes,
        this.totalPETaxes,
        this.totalQCTaxes,
        this.totalSKTaxes,
        this.totalYTTaxes,
      ];
      this.netIncomeArray = [
        netIncomeAB,
        netIncomeBC,
        netIncomeMB,
        netIncomeNB,
        netIncomeNL,
        netIncomeNT,
        netIncomeNS,
        netIncomeNU,
        netIncomeON,
        netIncomePE,
        netIncomeQC,
        netIncomeSK,
        netIncomeYT,
      ];
      this.avgTaxRateArray = [
        ABAvgRate,
        BCAvgRate,
        MBAvgRate,
        NBAvgRate,
        NLAvgRate,
        NTAvgRate,
        NSAvgRate,
        NUAvgRate,
        ONAvgRate,
        PEAvgRate,
        QCAvgRate,
        SKAvgRate,
        YTAvgRate,
      ];

      this.marginalTaxRateArray = this.calculateMarginalRates();

      //rank after-tax income array
      this.netIncomeArraySorted = this.netIncomeArray.slice(0);

      this.netIncomeArraySorted.sort(function(a, b) {
        return a - b;
      });

      this.netIncomeArraySorted.reverse();

      //generate array of after-tax income ranks
      var netIncomeRankOrder = [];
      var netIncomeArraySortedCopy = this.netIncomeArraySorted.slice(0);

      for (var i = 0; i < this.netIncomeArray.length; i++) {
        netIncomeRankOrder[i] = netIncomeArraySortedCopy.indexOf(
          this.netIncomeArray[i]
        );
        netIncomeArraySortedCopy[netIncomeRankOrder[i]] = "n/a";
      }

      //create new output arrays and sort by order of after-tax income
      this.labelArraySorted = this.sortArrayByRank(
        this.labelArray,
        netIncomeRankOrder
      );

      this.totalGrossIncomeArraySorted = this.sortArrayByRank(
        this.totalGrossIncomeArray,
        netIncomeRankOrder
      );

      this.federalTaxArraySorted = this.sortArrayByRank(
        this.federalTaxArray,
        netIncomeRankOrder
      );

      this.provincialTaxArraySorted = this.sortArrayByRank(
        this.provincialTaxArray,
        netIncomeRankOrder
      );
      this.CPPTaxArraySorted = this.sortArrayByRank(
        this.CPPTaxArray,
        netIncomeRankOrder
      );
      this.EITaxArraySorted = this.sortArrayByRank(
        this.EITaxArray,
        netIncomeRankOrder
      );
      this.QPPTaxArraySorted = this.sortArrayByRank(
        this.QPPTaxArray,
        netIncomeRankOrder
      );
      this.CPPQPPTaxArraySorted = this.sortArrayByRank(
        this.CPPQPPTaxArray,
        netIncomeRankOrder
      );
      this.QPIPTaxArraySorted = this.sortArrayByRank(
        this.QPIPTaxArray,
        netIncomeRankOrder
      );
      this.totalTaxesArraySorted = this.sortArrayByRank(
        this.totalTaxesArray,
        netIncomeRankOrder
      );
      this.avgTaxRateArraySorted = this.sortArrayByRank(
        this.avgTaxRateArray,
        netIncomeRankOrder
      );
      this.marginalTaxRateArraySorted = this.sortArrayByRank(
        this.marginalTaxRateArray,
        netIncomeRankOrder
      );

      //draw bar chart for after-tax income and total taxes paid
      //var ctx = this.afterTaxIncomeBarChart.getContext("2d");

      // this.chart = new Chart(ctx, {
      //   // The type of chart we want to create
      //   type: "horizontalBar",

      //   // The data for our dataset
      //   data: {
      //     labels: this.labelArraySorted,
      //     datasets: [
      //       {
      //         data: this.netIncomeArraySorted,
      //         backgroundColor: this.graphColourArray2,
      //         borderWidth: 1,
      //         borderColor: "#555555",
      //       },
      //     ],
      //   },

      //   //options for annual returns chart.js bar chart
      //   options: (this.outputBarChartOptions = {
      //     plugin_one_attribute: 1,
      //     maintainAspectRatio: false,

      //     tooltips: {
      //       // Include a dollar sign in the ticks and add comma formatting
      //       callbacks: {
      //         label: function(tooltipItem, data) {
      //           var label = data.datasets[tooltipItem.datasetIndex].label || "";

      //           if (label) {
      //             label += ": ";
      //           }
      //           label += "$" + Math.round(tooltipItem.xLabel).toLocaleString();
      //           return label;
      //         },
      //       },
      //     },

      //     scales: {
      //       xAxes: [
      //         {
      //           scaleLabel: {
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //             fontSize: 13,
      //           },

      //           ticks: {
      //             fontColor: "rgb(56,56,56)",

      //             min: 0,
      //             maxTicksLimit: 6,

      //             callback: function(value) {
      //               return "$" + value.toLocaleString();
      //             },
      //           },

      //           gridLines: {
      //             zeroLineColor: "rgb(56,56,56)",
      //             zeroLineWidth: 2,
      //           },
      //         },
      //       ],

      //       yAxes: [
      //         {
      //           ticks: {
      //             autoSkip: false,
      //             fontSize: 12,
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //           },

      //           scaleLabel: {
      //             display: true,
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //             fontSize: 13,
      //           },

      //           gridLines: {
      //             //zeroLineColor: "rgb(56,56,56)",
      //             //zeroLineWidth: 2,
      //           },
      //         },
      //       ],
      //     },

      //     legend: {
      //       display: false,
      //     },

      //     title: {
      //       display: true,
      //       text: "After-Tax Income",
      //       fontSize: 18,
      //       fontColor: "rgb(56,56,56)",
      //       padding: 8,
      //     },

      //     plugins: {
      //       datalabels: {
      //         formatter: function(value) {
      //           return "$" + Math.round(value).toLocaleString();
      //         },

      //         color: "#555555",

      //         anchor: "end",
      //         align: "end",

      //         clamp: true,
      //       },
      //     },
      //   }),
      // });

      // //draw bar chart for after-tax income and total taxes paid
      // var ctx2 = this.totalTaxBarChart.getContext("2d");

      // this.chart2 = new Chart(ctx2, {
      //   // The type of chart we want to create
      //   type: "horizontalBar",

      //   // The data for our dataset
      //   data: {
      //     labels: this.labelArraySorted,
      //     datasets: [
      //       {
      //         data: this.totalTaxesArraySorted,
      //         backgroundColor: this.this.graphColourArray2,
      //         borderWidth: 1,
      //         borderColor: "#555555",
      //       },
      //     ],
      //   },

      //   //options for annual returns chart.js bar chart
      //   options: (this.outputBarChartOptions = {
      //     plugin_one_attribute: 1,
      //     maintainAspectRatio: false,

      //     tooltips: {
      //       // Include a dollar sign in the ticks and add comma formatting
      //       callbacks: {
      //         label: function(tooltipItem, data) {
      //           var label = data.datasets[tooltipItem.datasetIndex].label || "";

      //           if (label) {
      //             label += ": ";
      //           }
      //           label += "$" + Math.round(tooltipItem.xLabel).toLocaleString();
      //           return label;
      //         },
      //       },
      //     },

      //     scales: {
      //       xAxes: [
      //         {
      //           scaleLabel: {
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //             fontSize: 13,
      //           },

      //           ticks: {
      //             fontColor: "rgb(56,56,56)",

      //             min: 0,
      //             maxTicksLimit: 6,

      //             callback: function(value) {
      //               return "$" + value.toLocaleString();
      //             },
      //           },

      //           gridLines: {
      //             zeroLineColor: "rgb(56,56,56)",
      //             zeroLineWidth: 2,
      //           },
      //         },
      //       ],

      //       yAxes: [
      //         {
      //           ticks: {
      //             autoSkip: false,
      //             fontSize: 12,
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //           },

      //           scaleLabel: {
      //             display: true,
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //             fontSize: 13,
      //           },

      //           gridLines: {
      //             //zeroLineColor: "rgb(56,56,56)",
      //             //zeroLineWidth: 2,
      //           },
      //         },
      //       ],
      //     },

      //     legend: {
      //       display: false,
      //     },

      //     title: {
      //       display: true,
      //       text: "Total Taxes Paid",
      //       fontSize: 18,
      //       fontColor: "rgb(56,56,56)",
      //       padding: 8,
      //     },

      //     plugins: {
      //       datalabels: {
      //         formatter: function(value) {
      //           return "$" + Math.round(value).toLocaleString();
      //         },

      //         color: "#555555",

      //         anchor: "end",
      //         align: "end",

      //         clamp: true,
      //       },
      //     },
      //   }),
      // });

      // //draw bar chart for after-tax income and total taxes paid
      // var ctx3 = this.avgTaxRateBarChart.getContext("2d");

      // this.chart3 = new Chart(ctx3, {
      //   // The type of chart we want to create
      //   type: "horizontalBar",

      //   // The data for our dataset
      //   data: {
      //     labels: this.labelArraySorted,
      //     datasets: [
      //       {
      //         data: this.avgTaxRateArraySorted,
      //         backgroundColor: this.graphColourArray2,
      //         borderWidth: 1,
      //         borderColor: "#555555",
      //       },
      //     ],
      //   },

      //   //options for annual returns chart.js bar chart
      //   options: (this.outputBarChartOptions = {
      //     plugin_one_attribute: 1,
      //     maintainAspectRatio: false,

      //     tooltips: {
      //       // Include a dollar sign in the ticks and add comma formatting
      //       callbacks: {
      //         label: function(tooltipItem, data) {
      //           var label = data.datasets[tooltipItem.datasetIndex].label || "";

      //           if (label) {
      //             label += ": ";
      //           }
      //           label +=
      //             (
      //               Math.round(tooltipItem.xLabel * 1000) / 10
      //             ).toLocaleString() + "%";
      //           return label;
      //         },
      //       },
      //     },

      //     scales: {
      //       xAxes: [
      //         {
      //           scaleLabel: {
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //             fontSize: 13,
      //           },

      //           ticks: {
      //             fontColor: "rgb(56,56,56)",

      //             min: 0,
      //             maxTicksLimit: 6,

      //             callback: function(value) {
      //               return Math.round(value * 100).toLocaleString() + "%";
      //             },
      //           },

      //           gridLines: {
      //             zeroLineColor: "rgb(56,56,56)",
      //             zeroLineWidth: 2,
      //           },
      //         },
      //       ],

      //       yAxes: [
      //         {
      //           ticks: {
      //             autoSkip: false,
      //             fontSize: 12,
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //           },

      //           scaleLabel: {
      //             display: true,
      //             fontColor: "rgb(56,56,56)",
      //             fontStyle: "bold",
      //             fontSize: 13,
      //           },

      //           gridLines: {
      //             //zeroLineColor: "rgb(56,56,56)",
      //             //zeroLineWidth: 2,
      //           },
      //         },
      //       ],
      //     },

      //     legend: {
      //       display: false,
      //     },

      //     title: {
      //       display: true,
      //       text: "Average Tax Rate",
      //       fontSize: 18,
      //       fontColor: "rgb(56,56,56)",
      //       padding: 8,
      //     },

      //     plugins: {
      //       datalabels: {
      //         formatter: function(value) {
      //           return (Math.round(value * 1000) / 10).toLocaleString() + "%";
      //         },

      //         clamp: true,

      //         color: "#555555",

      //         anchor: "end",
      //         align: "end",
      //       },
      //     },
      //   }),
      // });
    },

    refreshAnalysis1() {
      console.log("refresh analysis #1");
      // this.chart.destroy();
      // this.chart2.destroy();
      // this.chart3.destroy();
      this.getUserInputs();
      this.calculateTaxes();
      this.showOutputs();

      var tableRows = this.mainTable.getElementsByTagName("tr");
      var rowCount = tableRows.length;

      //delete all table rows ex header
      for (var x = rowCount - 1; x >= 0; x--) {
        // Ali changed > to >=
        this.mainTable.removeChild(tableRows[x]);
      }

      this.fillTable();
    },

    // refreshAnalysis2() {
    //   console.log("refresh analysis #2");
    //   this.chart4.destroy();
    //   this.getUserInputs();
    //   this.seekPreTaxIncome(this.netIncomeGoal);
    //   this.showOutputs2();
    // },

    // refreshAnalysis3() {
    //   console.log("refresh analysis #3");
    //   this.chart5.destroy();
    //   this.chart6.destroy();
    //   this.getUserInputs();
    //   this.provinceComparison(this.provinceA, this.provinceB);
    //   this.showOutputs3();
    // },

    sortArrayByRank(unsortedArray, rankOrder) {
      var sortedArray = [];
      // console.log("wookip1", unsortedArray);
      // console.log("wookip2", rankOrder);
      for (var i = 0; i < unsortedArray.length; i++) {
        if (rankOrder) {
          sortedArray[i] = unsortedArray[rankOrder.indexOf(i)];
        }
      }

      return sortedArray;
    },

    calculateMarginalRates() {
      var marginalArray = [];
      var currentTotalTaxes = this.totalTaxesArray.slice(0);

      this.totalTaxableIncome += 1000;

      this.calculateTaxes();

      var newTotalTaxes = [
        this.totalABTaxes,
        this.totalBCTaxes,
        this.totalMBTaxes,
        this.totalNBTaxes,
        this.totalNLTaxes,
        this.totalNTTaxes,
        this.totalNSTaxes,
        this.totalNUTaxes,
        this.totalONTaxes,
        this.totalPETaxes,
        this.totalQCTaxes,
        this.totalSKTaxes,
        this.totalYTTaxes,
      ];

      for (var i = 0; i < currentTotalTaxes.length; i++) {
        marginalArray[i] = (newTotalTaxes[i] - currentTotalTaxes[i]) / 1000;
      }

      this.totalTaxableIncome -= 1000;

      return marginalArray;
    },
    calculateCPP() {
      //CPP
      this.CPPTax = Math.max(
        Math.min(
          (this.employmentIncome - this.CPPExemption) *
            this.CPPContributionRate,
          this.CPPMaxContribution
        ),
        0
      );
      this.enhancedCPPTax =
        (this.enhancedCPPRate / this.CPPContributionRate) * this.CPPTax;
      this.baseCPPTax = this.CPPTax - this.enhancedCPPTax;
    },
    calculateQPP() {
      //QPP
      this.QPPTax = Math.max(
        Math.min(
          (this.employmentIncome - this.QPPExemption) *
            this.QPPContributionRate,
          this.QPPMaxContribution
        ),
        0
      );
      this.enhancedQPPTax =
        (this.enhancedQPPRate / this.QPPContributionRate) * this.QPPTax;
      this.baseQPPTax = this.QPPTax - this.enhancedQPPTax;
    },

    calculateEI() {
      //EI Rest of Canada
      this.EITaxROC = Math.max(
        Math.min(this.employmentIncome * this.EIROCRate, this.EIROCMax),
        0
      );

      //EI (Quebec)
      this.EITaxQC = Math.max(
        Math.min(this.employmentIncome * this.EIQCRate, this.EIQCMax),
        0
      );
    },

    calculateQPIP() {
      //QPIP
      if (this.employmentIncome < this.QPIPExemption) {
        this.QPIPTax = 0;
      } else {
        this.QPIPTax = Math.min(
          this.QPIPMaxContribution,
          this.employmentIncome * this.QPIPContributionRate
        );
      }
    },

    calculateOHIP() {
      //OHIP
      if (this.totalTaxableIncome < 20000) {
        this.OHIPContribution = 0;
      } else if (this.totalTaxableIncome < 25000) {
        this.OHIPContribution = (this.totalTaxableIncome - 20000) * 0.06;
      } else if (this.totalTaxableIncome < 36000) {
        this.OHIPContribution = 300;
      } else if (this.totalTaxableIncome < 38500) {
        this.OHIPContribution = 300 + (this.totalTaxableIncome - 36000) * 0.06;
      } else if (this.totalTaxableIncome < 48000) {
        this.OHIPContribution = 450;
      } else if (this.totalTaxableIncome < 48600) {
        this.OHIPContribution = 450 + (this.totalTaxableIncome - 48000) * 0.25;
      } else if (this.totalTaxableIncome < 72000) {
        this.OHIPContribution = 600;
      } else if (this.totalTaxableIncome < 72600) {
        this.OHIPContribution = 600 + (this.totalTaxableIncome - 72000) * 0.25;
      } else if (this.totalTaxableIncome < 200000) {
        this.OHIPContribution = 750;
      } else if (this.totalTaxableIncome < 200600) {
        this.OHIPContribution = 750 + (this.totalTaxableIncome - 200000) * 0.25;
      } else {
        this.OHIPContribution = 900;
      }
    },

    calculateFedTaxes() {
      //Federal Rest of Canada
      var fedBracket1Amount = 0;
      var fedBracket2Amount = 0;
      var fedBracket3Amount = 0;
      var fedBracket4Amount = 0;
      var fedBracket5Amount = 0;

      var fedBracket1Tax = 0;
      var fedBracket2Tax = 0;
      var fedBracket3Tax = 0;
      var fedBracket4Tax = 0;
      var fedBracket5Tax = 0;

      fedBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.federalBracket1
      );
      fedBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.federalBracket2) -
        fedBracket1Amount;
      fedBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.federalBracket3) -
        fedBracket1Amount -
        fedBracket2Amount;
      fedBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.federalBracket4) -
        fedBracket1Amount -
        fedBracket2Amount -
        fedBracket3Amount;
      fedBracket5Amount =
        Math.max(0, this.totalTaxableIncome) -
        fedBracket1Amount -
        fedBracket2Amount -
        fedBracket3Amount -
        fedBracket4Amount;

      fedBracket1Tax = fedBracket1Amount * this.federalRate1;
      fedBracket2Tax = fedBracket2Amount * this.federalRate2;
      fedBracket3Tax = fedBracket3Amount * this.federalRate3;
      fedBracket4Tax = fedBracket4Amount * this.federalRate4;
      fedBracket5Tax = fedBracket5Amount * this.federalRate5;

      var fedTaxSubtotal =
        fedBracket1Tax +
        fedBracket2Tax +
        fedBracket3Tax +
        fedBracket4Tax +
        fedBracket5Tax;

      this.enhancedBPA =
        this.maxEnhancedBPA -
        this.maxEnhancedBPA *
          Math.min(
            1,
            Math.max(
              0,
              (this.totalTaxableIncome - this.enhancedBPAThresholdStart) /
                (this.enhancedBPAThresholdEnd - this.enhancedBPAThresholdStart)
            )
          );

      this.totalFederalPersonalAmount =
        this.baseFederalPersonalAmount + this.enhancedBPA;

      var fedTaxCredits =
        (this.totalFederalPersonalAmount +
          this.baseCPPTax +
          this.EITaxROC +
          this.federalEmploymentAmount) *
          this.federalTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.federalEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.federalNonEligibleDivCredit;

      this.federalTaxROC = Math.max(fedTaxSubtotal - fedTaxCredits, 0);

      //Federal (Quebec)
      var fedTaxCreditsQC =
        (this.totalFederalPersonalAmount +
          this.baseQPPTax +
          this.QPIPTax +
          this.EITaxQC +
          this.federalEmploymentAmount) *
          this.federalTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.federalEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.federalNonEligibleDivCredit;

      this.federalTaxQC =
        Math.max(fedTaxSubtotal - fedTaxCreditsQC, 0) *
        (1 - this.QCAbatementRate);
      //console.log("federalTaxQC ", +this.federalTaxQC);
    },

    calculateAB() {
      //AB Provincial
      var ABBracket1Amount = 0;
      var ABBracket2Amount = 0;
      var ABBracket3Amount = 0;
      var ABBracket4Amount = 0;
      var ABBracket5Amount = 0;

      var ABBracket1Tax = 0;
      var ABBracket2Tax = 0;
      var ABBracket3Tax = 0;
      var ABBracket4Tax = 0;
      var ABBracket5Tax = 0;

      ABBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.ABBracket1
      );
      ABBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.ABBracket2) -
        ABBracket1Amount;
      ABBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.ABBracket3) -
        ABBracket1Amount -
        ABBracket2Amount;
      ABBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.ABBracket4) -
        ABBracket1Amount -
        ABBracket2Amount -
        ABBracket3Amount;
      ABBracket5Amount =
        Math.max(0, this.totalTaxableIncome) -
        ABBracket1Amount -
        ABBracket2Amount -
        ABBracket3Amount -
        ABBracket4Amount;

      ABBracket1Tax = ABBracket1Amount * this.ABRate1;
      ABBracket2Tax = ABBracket2Amount * this.ABRate2;
      ABBracket3Tax = ABBracket3Amount * this.ABRate3;
      ABBracket4Tax = ABBracket4Amount * this.ABRate4;
      ABBracket5Tax = ABBracket5Amount * this.ABRate5;

      var ABTaxSubtotal =
        ABBracket1Tax +
        ABBracket2Tax +
        ABBracket3Tax +
        ABBracket4Tax +
        ABBracket5Tax;

      var ABTaxCredits =
        (this.ABPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.ABTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.ABEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.ABNonEligibleDivCredit;

      this.ABTax = Math.max(ABTaxSubtotal - ABTaxCredits, 0);
    },
    calculateBC() {
      //BC Provincial
      var BCBracket1Amount = 0;
      var BCBracket2Amount = 0;
      var BCBracket3Amount = 0;
      var BCBracket4Amount = 0;
      var BCBracket5Amount = 0;
      var BCBracket6Amount = 0;
      var BCBracket7Amount = 0;

      var BCBracket1Tax = 0;
      var BCBracket2Tax = 0;
      var BCBracket3Tax = 0;
      var BCBracket4Tax = 0;
      var BCBracket5Tax = 0;
      var BCBracket6Tax = 0;
      var BCBracket7Tax = 0;

      BCBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.BCBracket1
      );
      BCBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.BCBracket2) -
        BCBracket1Amount;
      BCBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.BCBracket3) -
        BCBracket1Amount -
        BCBracket2Amount;
      BCBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.BCBracket4) -
        BCBracket1Amount -
        BCBracket2Amount -
        BCBracket3Amount;
      BCBracket5Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.BCBracket5) -
        BCBracket1Amount -
        BCBracket2Amount -
        BCBracket3Amount -
        BCBracket4Amount;
      BCBracket6Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.BCBracket6) -
        BCBracket1Amount -
        BCBracket2Amount -
        BCBracket3Amount -
        BCBracket4Amount -
        BCBracket5Amount;
      BCBracket7Amount =
        Math.max(0, this.totalTaxableIncome) -
        BCBracket1Amount -
        BCBracket2Amount -
        BCBracket3Amount -
        BCBracket4Amount -
        BCBracket5Amount -
        BCBracket6Amount;

      BCBracket1Tax = BCBracket1Amount * this.BCRate1;
      BCBracket2Tax = BCBracket2Amount * this.BCRate2;
      BCBracket3Tax = BCBracket3Amount * this.BCRate3;
      BCBracket4Tax = BCBracket4Amount * this.BCRate4;
      BCBracket5Tax = BCBracket5Amount * this.BCRate5;
      BCBracket6Tax = BCBracket6Amount * this.BCRate6;
      BCBracket7Tax = BCBracket7Amount * this.BCRate7;

      var BCTaxSubtotal =
        BCBracket1Tax +
        BCBracket2Tax +
        BCBracket3Tax +
        BCBracket4Tax +
        BCBracket5Tax +
        BCBracket6Tax +
        BCBracket7Tax;

      var BCTaxCredits =
        (this.BCPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.BCTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.BCEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.BCNonEligibleDivCredit;

      this.BCTax = Math.max(BCTaxSubtotal - BCTaxCredits, 0);
    },
    calculateMB() {
      //MB Provincial
      var MBBracket1Amount = 0;
      var MBBracket2Amount = 0;
      var MBBracket3Amount = 0;

      var MBBracket1Tax = 0;
      var MBBracket2Tax = 0;
      var MBBracket3Tax = 0;

      MBBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.MBBracket1
      );
      MBBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.MBBracket2) -
        MBBracket1Amount;
      MBBracket3Amount =
        Math.max(0, this.totalTaxableIncome) -
        MBBracket1Amount -
        MBBracket2Amount;

      MBBracket1Tax = MBBracket1Amount * this.MBRate1;
      MBBracket2Tax = MBBracket2Amount * this.MBRate2;
      MBBracket3Tax = MBBracket3Amount * this.MBRate3;

      var MBTaxSubtotal = MBBracket1Tax + MBBracket2Tax + MBBracket3Tax;

      var MBTaxCredits =
        (this.MBPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.MBTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.MBEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.MBNonEligibleDivCredit;

      this.MBTax = Math.max(MBTaxSubtotal - MBTaxCredits, 0);
    },

    calculateNB() {
      //NB Provincial
      var NBBracket1Amount = 0;
      var NBBracket2Amount = 0;
      var NBBracket3Amount = 0;
      var NBBracket4Amount = 0;
      var NBBracket5Amount = 0;

      var NBBracket1Tax = 0;
      var NBBracket2Tax = 0;
      var NBBracket3Tax = 0;
      var NBBracket4Tax = 0;
      var NBBracket5Tax = 0;

      NBBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.NBBracket1
      );
      NBBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NBBracket2) -
        NBBracket1Amount;
      NBBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NBBracket3) -
        NBBracket1Amount -
        NBBracket2Amount;
      NBBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NBBracket4) -
        NBBracket1Amount -
        NBBracket2Amount -
        NBBracket3Amount;
      NBBracket5Amount =
        Math.max(0, this.totalTaxableIncome) -
        NBBracket1Amount -
        NBBracket2Amount -
        NBBracket3Amount -
        NBBracket4Amount;

      NBBracket1Tax = NBBracket1Amount * this.NBRate1;
      NBBracket2Tax = NBBracket2Amount * this.NBRate2;
      NBBracket3Tax = NBBracket3Amount * this.NBRate3;
      NBBracket4Tax = NBBracket4Amount * this.NBRate4;
      NBBracket5Tax = NBBracket5Amount * this.NBRate5;

      var NBTaxSubtotal =
        NBBracket1Tax +
        NBBracket2Tax +
        NBBracket3Tax +
        NBBracket4Tax +
        NBBracket5Tax;

      var NBTaxCredits =
        (this.NBPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.NBTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.NBEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.NBNonEligibleDivCredit;

      this.NBTax = Math.max(NBTaxSubtotal - NBTaxCredits, 0);
    },
    calculateNL() {
      //NL Provincial
      var NLBracket1Amount = 0;
      var NLBracket2Amount = 0;
      var NLBracket3Amount = 0;
      var NLBracket4Amount = 0;
      var NLBracket5Amount = 0;

      var NLBracket1Tax = 0;
      var NLBracket2Tax = 0;
      var NLBracket3Tax = 0;
      var NLBracket4Tax = 0;
      var NLBracket5Tax = 0;

      NLBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.NLBracket1
      );
      NLBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NLBracket2) -
        NLBracket1Amount;
      NLBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NLBracket3) -
        NLBracket1Amount -
        NLBracket2Amount;
      NLBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NLBracket4) -
        NLBracket1Amount -
        NLBracket2Amount -
        NLBracket3Amount;
      NLBracket5Amount =
        Math.max(0, this.totalTaxableIncome) -
        NLBracket1Amount -
        NLBracket2Amount -
        NLBracket3Amount -
        NLBracket4Amount;

      NLBracket1Tax = NLBracket1Amount * this.NLRate1;
      NLBracket2Tax = NLBracket2Amount * this.NLRate2;
      NLBracket3Tax = NLBracket3Amount * this.NLRate3;
      NLBracket4Tax = NLBracket4Amount * this.NLRate4;
      NLBracket5Tax = NLBracket5Amount * this.NLRate5;

      var NLTaxSubtotal =
        NLBracket1Tax +
        NLBracket2Tax +
        NLBracket3Tax +
        NLBracket4Tax +
        NLBracket5Tax;

      var NLTaxCredits =
        (this.NLPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.NLTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.NLEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.NLNonEligibleDivCredit;

      this.NLTax = Math.max(NLTaxSubtotal - NLTaxCredits, 0);
    },
    calculateNT() {
      //NT Territorial
      var NTBracket1Amount = 0;
      var NTBracket2Amount = 0;
      var NTBracket3Amount = 0;
      var NTBracket4Amount = 0;

      var NTBracket1Tax = 0;
      var NTBracket2Tax = 0;
      var NTBracket3Tax = 0;
      var NTBracket4Tax = 0;

      NTBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.NTBracket1
      );
      NTBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NTBracket2) -
        NTBracket1Amount;
      NTBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NTBracket3) -
        NTBracket1Amount -
        NTBracket2Amount;
      NTBracket4Amount =
        Math.max(0, this.totalTaxableIncome) -
        NTBracket1Amount -
        NTBracket2Amount -
        NTBracket3Amount;

      NTBracket1Tax = NTBracket1Amount * this.NTRate1;
      NTBracket2Tax = NTBracket2Amount * this.NTRate2;
      NTBracket3Tax = NTBracket3Amount * this.NTRate3;
      NTBracket4Tax = NTBracket4Amount * this.NTRate4;

      var NTTaxSubtotal =
        NTBracket1Tax + NTBracket2Tax + NTBracket3Tax + NTBracket4Tax;

      var NTTaxCredits =
        (this.NTPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.NTTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.NTEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.NTNonEligibleDivCredit;

      this.NTTax = Math.max(NTTaxSubtotal - NTTaxCredits, 0);
    },
    calculateNS() {
      //NS Provincial
      var NSBracket1Amount = 0;
      var NSBracket2Amount = 0;
      var NSBracket3Amount = 0;
      var NSBracket4Amount = 0;
      var NSBracket5Amount = 0;

      var NSBracket1Tax = 0;
      var NSBracket2Tax = 0;
      var NSBracket3Tax = 0;
      var NSBracket4Tax = 0;
      var NSBracket5Tax = 0;

      NSBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.NSBracket1
      );
      NSBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NSBracket2) -
        NSBracket1Amount;
      NSBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NSBracket3) -
        NSBracket1Amount -
        NSBracket2Amount;
      NSBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NSBracket4) -
        NSBracket1Amount -
        NSBracket2Amount -
        NSBracket3Amount;
      NSBracket5Amount =
        Math.max(0, this.totalTaxableIncome) -
        NSBracket1Amount -
        NSBracket2Amount -
        NSBracket3Amount -
        NSBracket4Amount;

      NSBracket1Tax = NSBracket1Amount * this.NSRate1;
      NSBracket2Tax = NSBracket2Amount * this.NSRate2;
      NSBracket3Tax = NSBracket3Amount * this.NSRate3;
      NSBracket4Tax = NSBracket4Amount * this.NSRate4;
      NSBracket5Tax = NSBracket5Amount * this.NSRate5;

      var NSTaxSubtotal =
        NSBracket1Tax +
        NSBracket2Tax +
        NSBracket3Tax +
        NSBracket4Tax +
        NSBracket5Tax;

      this.NSEnhancedPersonalAmount = Math.max(
        0,
        3000 - Math.max(0, (this.totalTaxableIncome - 25000) * 0.06)
      );
      this.NSTotalPersonalAmount =
        this.NSPersonalAmount + this.NSEnhancedPersonalAmount;

      var NSTaxCredits =
        (this.NSTotalPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.NSTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.NSEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.NSNonEligibleDivCredit;

      this.NSTax = Math.max(NSTaxSubtotal - NSTaxCredits, 0);
    },
    calculateNU() {
      //NU Territorial
      var NUBracket1Amount = 0;
      var NUBracket2Amount = 0;
      var NUBracket3Amount = 0;
      var NUBracket4Amount = 0;

      var NUBracket1Tax = 0;
      var NUBracket2Tax = 0;
      var NUBracket3Tax = 0;
      var NUBracket4Tax = 0;

      NUBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.NUBracket1
      );
      NUBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NUBracket2) -
        NUBracket1Amount;
      NUBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.NUBracket3) -
        NUBracket1Amount -
        NUBracket2Amount;
      NUBracket4Amount =
        Math.max(0, this.totalTaxableIncome) -
        NUBracket1Amount -
        NUBracket2Amount -
        NUBracket3Amount;

      NUBracket1Tax = NUBracket1Amount * this.NURate1;
      NUBracket2Tax = NUBracket2Amount * this.NURate2;
      NUBracket3Tax = NUBracket3Amount * this.NURate3;
      NUBracket4Tax = NUBracket4Amount * this.NURate4;

      var NUTaxSubtotal =
        NUBracket1Tax + NUBracket2Tax + NUBracket3Tax + NUBracket4Tax;

      var NUTaxCredits =
        (this.NUPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.NUTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.NUEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.NUNonEligibleDivCredit;

      this.NUTax = Math.max(NUTaxSubtotal - NUTaxCredits, 0);
    },
    calculateON() {
      //ON Provincial
      var ONBracket1Amount = 0;
      var ONBracket2Amount = 0;
      var ONBracket3Amount = 0;
      var ONBracket4Amount = 0;
      var ONBracket5Amount = 0;

      var ONBracket1Tax = 0;
      var ONBracket2Tax = 0;
      var ONBracket3Tax = 0;
      var ONBracket4Tax = 0;
      var ONBracket5Tax = 0;

      ONBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.ONBracket1
      );
      ONBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.ONBracket2) -
        ONBracket1Amount;
      ONBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.ONBracket3) -
        ONBracket1Amount -
        ONBracket2Amount;
      ONBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.ONBracket4) -
        ONBracket1Amount -
        ONBracket2Amount -
        ONBracket3Amount;
      ONBracket5Amount =
        Math.max(0, this.totalTaxableIncome) -
        ONBracket1Amount -
        ONBracket2Amount -
        ONBracket3Amount -
        ONBracket4Amount;

      ONBracket1Tax = ONBracket1Amount * this.ONRate1;
      ONBracket2Tax = ONBracket2Amount * this.ONRate2;
      ONBracket3Tax = ONBracket3Amount * this.ONRate3;
      ONBracket4Tax = ONBracket4Amount * this.ONRate4;
      ONBracket5Tax = ONBracket5Amount * this.ONRate5;

      var ONTaxCredits1 =
        (this.ONPersonalAmount + this.baseCPPTax + this.EITaxROC) *
        this.ONTaxCreditRate;

      var ONTaxBeforeSurtax =
        ONBracket1Tax +
        ONBracket2Tax +
        ONBracket3Tax +
        ONBracket4Tax +
        ONBracket5Tax -
        ONTaxCredits1;

      var ONSurtax =
        Math.max(0, ONTaxBeforeSurtax - this.ONSurtax1) * this.ONSurtax1Rate +
        Math.max(0, ONTaxBeforeSurtax - this.ONSurtax2) * this.ONSurtax2Rate;

      var ONTaxCredits2 =
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.ONEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.ONNonEligibleDivCredit;

      this.ONTax =
        Math.max(ONTaxBeforeSurtax + ONSurtax - ONTaxCredits2, 0) +
        this.OHIPContribution;
    },

    calculatePE() {
      //PE Provincial
      var PEBracket1Amount = 0;
      var PEBracket2Amount = 0;
      var PEBracket3Amount = 0;

      var PEBracket1Tax = 0;
      var PEBracket2Tax = 0;
      var PEBracket3Tax = 0;

      PEBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.PEBracket1
      );
      PEBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.PEBracket2) -
        PEBracket1Amount;
      PEBracket3Amount =
        Math.max(0, this.totalTaxableIncome) -
        PEBracket1Amount -
        PEBracket2Amount;

      PEBracket1Tax = PEBracket1Amount * this.PERate1;
      PEBracket2Tax = PEBracket2Amount * this.PERate2;
      PEBracket3Tax = PEBracket3Amount * this.PERate3;

      var PETaxSubtotal = PEBracket1Tax + PEBracket2Tax + PEBracket3Tax;

      var PETaxCredits =
        (this.PEPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.PETaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.PEEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.PENonEligibleDivCredit;

      var PETaxBeforeSurtax = Math.max(PETaxSubtotal - PETaxCredits, 0);

      var PESurtax =
        Math.max(PETaxBeforeSurtax - this.PESurtax1, 0) * this.PESurtax1Rate;

      this.PETax = PETaxBeforeSurtax + PESurtax;
    },

    calculateQC() {
      //QC Provincial
      var QCBracket1Amount = 0;
      var QCBracket2Amount = 0;
      var QCBracket3Amount = 0;
      var QCBracket4Amount = 0;

      var QCBracket1Tax = 0;
      var QCBracket2Tax = 0;
      var QCBracket3Tax = 0;
      var QCBracket4Tax = 0;

      QCBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.QCBracket1
      );
      QCBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.QCBracket2) -
        QCBracket1Amount;
      QCBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.QCBracket3) -
        QCBracket1Amount -
        QCBracket2Amount;
      QCBracket4Amount =
        Math.max(0, this.totalTaxableIncome) -
        QCBracket1Amount -
        QCBracket2Amount -
        QCBracket3Amount;

      QCBracket1Tax = QCBracket1Amount * this.QCRate1;
      QCBracket2Tax = QCBracket2Amount * this.QCRate2;
      QCBracket3Tax = QCBracket3Amount * this.QCRate3;
      QCBracket4Tax = QCBracket4Amount * this.QCRate4;

      var QCTaxSubtotal =
        QCBracket1Tax + QCBracket2Tax + QCBracket3Tax + QCBracket4Tax;

      var QCTaxCredits =
        this.QCPersonalAmount * this.QCTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.QCEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.QCNonEligibleDivCredit;

      this.QCTax = Math.max(QCTaxSubtotal - QCTaxCredits, 0);
    },

    calculateSK() {
      //SK Provincial
      var SKBracket1Amount = 0;
      var SKBracket2Amount = 0;
      var SKBracket3Amount = 0;

      var SKBracket1Tax = 0;
      var SKBracket2Tax = 0;
      var SKBracket3Tax = 0;

      SKBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.SKBracket1
      );
      SKBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.SKBracket2) -
        SKBracket1Amount;
      SKBracket3Amount =
        Math.max(0, this.totalTaxableIncome) -
        SKBracket1Amount -
        SKBracket2Amount;

      SKBracket1Tax = SKBracket1Amount * this.SKRate1;
      SKBracket2Tax = SKBracket2Amount * this.SKRate2;
      SKBracket3Tax = SKBracket3Amount * this.SKRate3;

      var SKTaxSubtotal = SKBracket1Tax + SKBracket2Tax + SKBracket3Tax;

      var SKTaxCredits =
        (this.SKPersonalAmount + this.baseCPPTax + this.EITaxROC) *
          this.SKTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.SKEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.SKNonEligibleDivCredit;

      this.SKTax = Math.max(SKTaxSubtotal - SKTaxCredits, 0);
    },

    calculateYT() {
      //YT Territorial
      var YTBracket1Amount = 0;
      var YTBracket2Amount = 0;
      var YTBracket3Amount = 0;
      var YTBracket4Amount = 0;
      var YTBracket5Amount = 0;

      var YTBracket1Tax = 0;
      var YTBracket2Tax = 0;
      var YTBracket3Tax = 0;
      var YTBracket4Tax = 0;
      var YTBracket5Tax = 0;

      YTBracket1Amount = Math.min(
        Math.max(0, this.totalTaxableIncome),
        this.YTBracket1
      );
      YTBracket2Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.YTBracket2) -
        YTBracket1Amount;
      YTBracket3Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.YTBracket3) -
        YTBracket1Amount -
        YTBracket2Amount;
      YTBracket4Amount =
        Math.min(Math.max(0, this.totalTaxableIncome), this.YTBracket4) -
        YTBracket1Amount -
        YTBracket2Amount -
        YTBracket3Amount;
      YTBracket5Amount =
        Math.max(0, this.totalTaxableIncome) -
        YTBracket1Amount -
        YTBracket2Amount -
        YTBracket3Amount -
        YTBracket4Amount;

      YTBracket1Tax = YTBracket1Amount * this.YTRate1;
      YTBracket2Tax = YTBracket2Amount * this.YTRate2;
      YTBracket3Tax = YTBracket3Amount * this.YTRate3;
      YTBracket4Tax = YTBracket4Amount * this.YTRate4;
      YTBracket5Tax = YTBracket5Amount * this.YTRate5;

      var YTTaxSubtotal =
        YTBracket1Tax +
        YTBracket2Tax +
        YTBracket3Tax +
        YTBracket4Tax +
        YTBracket5Tax;

      var YTTaxCredits =
        (this.YTPersonalAmount +
          this.baseCPPTax +
          this.EITaxROC +
          this.federalEmploymentAmount) *
          this.YTTaxCreditRate +
        this.eligibleDividends *
          (1 + this.eligibleGrossUp) *
          this.YTEligibleDivCredit +
        this.nonEligibleDividends *
          (1 + this.nonEligibleGrossUp) *
          this.YTNonEligibleDivCredit;

      this.YTTax = Math.max(YTTaxSubtotal - YTTaxCredits, 0);
    },

    // seekPreTaxIncome(netIncomeGoal) {
    //   var maxLoops = 5000;
    //   var incomeSeekIncrement = 500;

    //   //reset all assumptions to zero
    //   this.employmentIncome = 0;
    //   this.capitalGains = 0;
    //   this.eligibleDividends = 0;
    //   this.nonEligibleDividends = 0;
    //   this.otherIncome = 0;
    //   this.RRSPContribution = 0;
    //   this.totalTaxableIncome = 0;

    //   var ABTestNetIncome = 0;
    //   var BCTestNetIncome = 0;
    //   var MBTestNetIncome = 0;
    //   var NBTestNetIncome = 0;
    //   var NLTestNetIncome = 0;
    //   var NSTestNetIncome = 0;
    //   var NTTestNetIncome = 0;
    //   var NUTestNetIncome = 0;
    //   var ONTestNetIncome = 0;
    //   var PETestNetIncome = 0;
    //   var QCTestNetIncome = 0;
    //   var SKTestNetIncome = 0;
    //   var YTTestNetIncome = 0;

    //   var ABGrossIncome = 0;
    //   var BCGrossIncome = 0;
    //   var MBGrossIncome = 0;
    //   var NBGrossIncome = 0;
    //   var NLGrossIncome = 0;
    //   var NSGrossIncome = 0;
    //   var NTGrossIncome = 0;
    //   var NUGrossIncome = 0;
    //   var ONGrossIncome = 0;
    //   var PEGrossIncome = 0;
    //   var QCGrossIncome = 0;
    //   var SKGrossIncome = 0;
    //   var YTGrossIncome = 0;

    //   //initialize switches for each province (set to 1 if net income goal has been reached)
    //   var ABSwitch = 0;
    //   var BCSwitch = 0;
    //   var MBSwitch = 0;
    //   var NBSwitch = 0;
    //   var NLSwitch = 0;
    //   var NTSwitch = 0;
    //   var NSSwitch = 0;
    //   var NUSwitch = 0;
    //   var ONSwitch = 0;
    //   var PESwitch = 0;
    //   var QCSwitch = 0;
    //   var SKSwitch = 0;
    //   var YTSwitch = 0;

    //   var switchTotal = 0;

    //   for (var i = 0; i < maxLoops; i++) {
    //     this.employmentIncome = i * incomeSeekIncrement;

    //     this.calculateCPP();
    //     this.calculateQPP();

    //     this.totalTaxableIncome = i * incomeSeekIncrement - this.enhancedCPPTax;

    //     this.calculateEI();
    //     this.calculateQPIP();
    //     this.calculateOHIP();

    //     this.calculateFedTaxes();

    //     //AB
    //     if (ABSwitch == 0) {
    //       this.calculateAB();
    //       ABTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.BTax -
    //         this.CPPTax -
    //         this.EITaxROC;

    //       if (ABTestNetIncome >= netIncomeGoal) {
    //         ABSwitch = 1;
    //         ABGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //BC
    //     if (BCSwitch == 0) {
    //       this.calculateBC();
    //       BCTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.BCTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (BCTestNetIncome >= netIncomeGoal) {
    //         BCSwitch = 1;
    //         BCGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //MB
    //     if (MBSwitch == 0) {
    //       this.calculateMB();
    //       MBTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.MBTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (MBTestNetIncome >= netIncomeGoal) {
    //         MBSwitch = 1;
    //         MBGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //NB
    //     if (NBSwitch == 0) {
    //       this.calculateNB();
    //       NBTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.NBTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (NBTestNetIncome >= netIncomeGoal) {
    //         NBSwitch = 1;
    //         NBGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //NL
    //     if (NLSwitch == 0) {
    //       this.calculateNL();
    //       NLTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.NLTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (NLTestNetIncome >= netIncomeGoal) {
    //         NLSwitch = 1;
    //         NLGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //NT
    //     if (NTSwitch == 0) {
    //       this.calculateNT();
    //       NTTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.NTTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (NTTestNetIncome >= netIncomeGoal) {
    //         NTSwitch = 1;
    //         NTGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //NS
    //     if (NSSwitch == 0) {
    //       this.calculateNS();
    //       NSTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.NSTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (NSTestNetIncome >= netIncomeGoal) {
    //         NSSwitch = 1;
    //         NSGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //NU
    //     if (NUSwitch == 0) {
    //       this.calculateNU();
    //       NUTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.NUTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (NUTestNetIncome >= netIncomeGoal) {
    //         NUSwitch = 1;
    //         NUGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //ON
    //     if (ONSwitch == 0) {
    //       this.calculateON();
    //       ONTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.ONTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (ONTestNetIncome >= netIncomeGoal) {
    //         ONSwitch = 1;
    //         ONGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //PE
    //     if (PESwitch == 0) {
    //       this.calculatePE();
    //       PETestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.PETax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (PETestNetIncome >= netIncomeGoal) {
    //         PESwitch = 1;
    //         PEGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //QC
    //     if (QCSwitch == 0) {
    //       this.calculateQC();
    //       QCTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxQC -
    //         this.QCTax -
    //         this.QPPTax -
    //         this.EITaxQC -
    //         this.QPIPTax;
    //       if (QCTestNetIncome >= netIncomeGoal) {
    //         QCSwitch = 1;
    //         QCGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //SK
    //     if (SKSwitch == 0) {
    //       this.calculateSK();
    //       SKTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.SKTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (SKTestNetIncome >= netIncomeGoal) {
    //         SKSwitch = 1;
    //         SKGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     //YT
    //     if (YTSwitch == 0) {
    //       this.calculateYT();
    //       YTTestNetIncome =
    //         this.employmentIncome -
    //         this.federalTaxROC -
    //         this.YTTax -
    //         this.CPPTax -
    //         this.EITaxROC;
    //       if (YTTestNetIncome >= netIncomeGoal) {
    //         YTSwitch = 1;
    //         YTGrossIncome = this.employmentIncome;
    //       }
    //     }

    //     switchTotal =
    //       ABSwitch +
    //       BCSwitch +
    //       MBSwitch +
    //       NBSwitch +
    //       NLSwitch +
    //       NTSwitch +
    //       NSSwitch +
    //       NUSwitch +
    //       ONSwitch +
    //       PESwitch +
    //       QCSwitch +
    //       SKSwitch +
    //       YTSwitch;

    //     if (switchTotal == 13) {
    //       break;
    //     }
    //   }

    //   this.grossIncomeArray = [
    //     ABGrossIncome,
    //     BCGrossIncome,
    //     MBGrossIncome,
    //     NBGrossIncome,
    //     NLGrossIncome,
    //     NTGrossIncome,
    //     NSGrossIncome,
    //     NUGrossIncome,
    //     ONGrossIncome,
    //     PEGrossIncome,
    //     QCGrossIncome,
    //     SKGrossIncome,
    //     YTGrossIncome,
    //   ];
    // },

    // showOutputs2() {
    //   //rank gross income array
    //   var grossIncomeArraySorted = this.grossIncomeArray.slice(0);

    //   grossIncomeArraySorted.sort(function(a, b) {
    //     return a - b;
    //   });

    //   //generate array of after-tax income ranks
    //   var grossIncomeRankOrder = [];
    //   var grossIncomeArraySortedCopy = grossIncomeArraySorted.slice(0);

    //   for (var i = 0; i < this.grossIncomeArray.length; i++) {
    //     grossIncomeRankOrder[i] = grossIncomeArraySortedCopy.indexOf(
    //       this.grossIncomeArray[i]
    //     );
    //     grossIncomeArraySortedCopy[grossIncomeRankOrder[i]] = "n/a";
    //   }

    //   //generate label array sorted by gross income order
    //   var labelArraySorted2 = this.sortArrayByRank(
    //     this.labelArray,
    //     this.grossIncomeRankOrder
    //   );
    //   console.log(labelArraySorted2);

    //draw bar chart for after-tax income and total taxes paid
    //var ctx4 = this.grossIncomeBarChart.getContext("2d");

    // this.chart4 = new Chart(ctx4, {
    //   // The type of chart we want to create
    //   type: "horizontalBar",

    //   // The data for our dataset
    //   data: {
    //     labels: labelArraySorted2,
    //     datasets: [
    //       {
    //         data: this.grossIncomeArraySorted,
    //         backgroundColor: this.graphColourArray,
    //         borderWidth: 1,
    //         borderColor: "#555555",
    //       },
    //     ],
    //   },

    //   //options for chart.js bar chart
    //   options: (this.outputBarChartOptions = {
    //     plugin_one_attribute: 1,
    //     maintainAspectRatio: false,

    //     tooltips: {
    //       // Include a dollar sign in the ticks and add comma formatting
    //       callbacks: {
    //         label: function(tooltipItem, data) {
    //           var label = data.datasets[tooltipItem.datasetIndex].label || "";

    //           if (label) {
    //             label += ": ";
    //           }
    //           label += "$" + Math.round(tooltipItem.xLabel).toLocaleString();
    //           return label;
    //         },
    //       },
    //     },

    //     scales: {
    //       xAxes: [
    //         {
    //           scaleLabel: {
    //             fontColor: "rgb(56,56,56)",
    //             fontStyle: "bold",
    //             fontSize: 13,
    //           },

    //           ticks: {
    //             fontColor: "rgb(56,56,56)",

    //             min: 0,
    //             maxTicksLimit: 6,

    //             callback: function(value) {
    //               return "$" + value.toLocaleString();
    //             },
    //           },

    //           gridLines: {
    //             zeroLineColor: "rgb(56,56,56)",
    //             zeroLineWidth: 2,
    //           },
    //         },
    //       ],

    //       yAxes: [
    //         {
    //           ticks: {
    //             autoSkip: false,
    //             fontSize: 12,
    //             fontColor: "rgb(56,56,56)",
    //             fontStyle: "bold",
    //           },

    //           scaleLabel: {
    //             display: true,
    //             fontColor: "rgb(56,56,56)",
    //             fontStyle: "bold",
    //             fontSize: 13,
    //           },

    //           gridLines: {
    //             //zeroLineColor: "rgb(56,56,56)",
    //             //zeroLineWidth: 2,
    //           },
    //         },
    //       ],
    //     },

    //     legend: {
    //       display: false,
    //     },

    //     title: {
    //       display: true,
    //       text:
    //         "Pre-Tax Employment Income Needed to Generate $" +
    //         Math.round(this.netIncomeGoal).toLocaleString() +
    //         " of After-Tax Income",
    //       fontSize: 15,
    //       fontColor: "rgb(56,56,56)",
    //       padding: 8,
    //     },

    //     plugins: {
    //       datalabels: {
    //         formatter: function(value) {
    //           return "$" + Math.round(value).toLocaleString();
    //         },

    //         color: "#555555",

    //         anchor: "end",
    //         align: "end",

    //         clamp: true,
    //       },
    //     },
    //   }),
    // });
    // },

    // provinceComparison(a, b) {
    //   var aString = String("calculate" + a);
    //   var bString = String("calculate" + b);

    //   var aString2 = String(a + "Tax");
    //   var bString2 = String(b + "Tax");

    //   //set defaults
    //   this.employmentIncome = 0;
    //   this.capitalGains = 0;
    //   this.eligibleDividends = 0;
    //   this.nonEligibleDividends = 0;
    //   this.otherIncome = 0;
    //   this.RRSPContribution = 0;
    //   this.totalTaxableIncome = 0;

    //   //arrays to be charted
    //   this.grossIncomeLabelArray = [];
    //   this.provinceANetIncomeArray = [];
    //   this.provinceBNetIncomeArray = [];

    //   for (var i = 0; i <= 300; i++) {
    //     this.employmentIncome = i * 1000;

    //     this.calculateCPP();
    //     this.calculateQPP();

    //     this.totalTaxableIncome = this.employmentIncome - this.enhancedCPPTax;

    //     this.calculateEI();
    //     this.calculateQPIP();
    //     this.calculateOHIP();
    //     this.calculateFedTaxes();

    //     this.executeFunctionByName(aString, window);
    //     this.executeFunctionByName(bString, window);

    //     this.grossIncomeLabelArray[i] = this.employmentIncome;

    //     if (a == "QC") {
    //       this.provinceANetIncomeArray[i] =
    //         this.employmentIncome -
    //         this.federalTaxQC -
    //         this.QPPTax -
    //         this.EITaxQC -
    //         this.QPIPTax -
    //         this.QCTax;
    //     } else {
    //       this.provinceANetIncomeArray[i] = eval(
    //         "this.employmentIncome - this.federalTaxROC - this.CPPTax - this.EITaxROC - " +
    //           aString2
    //       );
    //     }

    //     if (b == "QC") {
    //       this.provinceBNetIncomeArray[i] =
    //         this.employmentIncome -
    //         this.federalTaxQC -
    //         this.QPPTax -
    //         this.EITaxQC -
    //         this.QPIPTax -
    //         this.QCTax;
    //     } else {
    //       this.provinceBNetIncomeArray[i] = eval(
    //         "this.employmentIncome - this.federalTaxROC - this.CPPTax - this.EITaxROC - " +
    //           bString2
    //       );
    //     }

    //     this.provinceDeltaArray[i] =
    //       this.provinceANetIncomeArray[i] - this.provinceBNetIncomeArray[i];
    //   }
    // },

    // showOutputs3() {
    //   this.analysis3Para.innerHTML =
    //     "For the chart below, positive values indicate that after-tax income is higher in " +
    //     this.provinceA +
    //     ", while negative values mean that after-tax income is higher in " +
    //     this.provinceB +
    //     ".";

    //var ctx5 = document.getElementById("provinceChart").getContext("2d");

    // this.chart5 = new Chart(ctx5, {
    //   // The type of chart we want to create
    //   type: "line",

    //   // The data for our dataset
    //   data: {
    //     labels: this.grossIncomeLabelArray,
    //     datasets: [
    //       {
    //         label: this.provinceA,
    //         borderColor: "#0071B3",
    //         pointBackgroundColor: "#0071B3",
    //         fill: false,
    //         data: this.provinceANetIncomeArray,
    //         pointHitRadius: 5,
    //         radius: 0,
    //         lineTension: 0,
    //       },

    //       {
    //         label: this.provinceB,
    //         borderColor: "#B31B20",
    //         pointBackgroundColor: "#B31B20",
    //         fill: false,
    //         data: this.rovinceBNetIncomeArray,
    //         pointHitRadius: 5,
    //         radius: 0,
    //         lineTension: 0,
    //       },
    //     ],
    //   },

    //   // Configuration options go here
    //   options: {
    //     maintainAspectRatio: false,

    //     tooltips: {
    //       // Include a dollar sign in the ticks and add comma formatting
    //       callbacks: {
    //         label: function(tooltipItem, data) {
    //           var label = data.datasets[tooltipItem.datasetIndex].label || "";

    //           if (label) {
    //             label += ": ";
    //           }
    //           label += "$" + Math.round(tooltipItem.yLabel).toLocaleString();
    //           return label;
    //         },
    //       },
    //     },

    //     scales: {
    //       yAxes: [
    //         {
    //           ticks: {
    //             // Include a dollar sign in the ticks and add comma formatting
    //             callback: function(value) {
    //               return "$" + Math.round(value).toLocaleString();
    //             },

    //             fontColor: "rgb(56,56,56)",
    //             max: 200000,
    //             min: 0,
    //           },

    //           scaleLabel: {
    //             display: true,
    //             labelString: "After-Tax Income",
    //             fontColor: "rgb(56,56,56)",
    //             fontStyle: "bold",
    //             fontSize: 15,
    //           },

    //           gridLines: {
    //             drawTicks: false,
    //             zeroLineColor: "rgb(56,56,56)",
    //             zeroLineWidth: 2,
    //           },
    //         },
    //       ],

    //       xAxes: [
    //         {
    //           ticks: {
    //             fontColor: "rgb(56,56,56)",
    //             maxTicksLimit: 15,
    //             min: 0,
    //             max: 300000,
    //             minRotation: 90,
    //             maxRotation: 90,
    //             callback: function(value) {
    //               return "$" + Math.round(value).toLocaleString();
    //             },
    //           },

    //           scaleLabel: {
    //             display: true,
    //             labelString: "Pre-Tax Employment Income",
    //             fontColor: "rgb(56,56,56)",
    //             fontStyle: "bold",
    //             fontSize: 15,
    //           },

    //           gridLines: {
    //             drawTicks: false,
    //             zeroLineColor: "rgb(56,56,56)",
    //             zeroLineWidth: 2,
    //           },
    //         },
    //       ],
    //     },

    //     legend: {
    //       labels: {
    //         fontColor: "rgb(56,56,56)",
    //         boxWidth: 13,
    //         padding: 10,
    //       },
    //     },

    //     title: {
    //       display: true,
    //       text:
    //         "After-Tax Income: " + this.provinceA + " vs " + this.provinceB,
    //       fontSize: 18,
    //       fontColor: "rgb(56,56,56)",
    //       padding: 2,
    //     },

    //     plugins: {
    //       datalabels: {
    //         formatter: function(value) {
    //           return "$" + Math.round(value).toLocaleString();
    //         },

    //         display: false,
    //       },
    //     },
    //   },
    // });

    // var ctx6 = document.getElementById("provinceDeltaChart").getContext("2d");

    // this.chart6 = new Chart(ctx6, {
    //   // The type of chart we want to create
    //   type: "line",

    //   // The data for our dataset
    //   data: {
    //     labels: this.grossIncomeLabelArray,
    //     datasets: [
    //       {
    //         label: "Difference in after-tax income",
    //         borderColor: "#e71a8c",
    //         pointBackgroundColor: "#e71a8c",
    //         fill: false,
    //         data: this.provinceDeltaArray,
    //         pointHitRadius: 5,
    //         radius: 0,
    //         lineTension: 0,
    //       },
    //     ],
    //   },

    //   // Configuration options go here
    //   options: {
    //     maintainAspectRatio: false,

    //     tooltips: {
    //       // Include a dollar sign in the ticks and add comma formatting
    //       callbacks: {
    //         label: function(tooltipItem, data) {
    //           var label = data.datasets[tooltipItem.datasetIndex].label || "";

    //           if (label) {
    //             label += ": ";
    //           }

    //           if (tooltipItem.yLabel >= 0) {
    //             label +=
    //               "$" + Math.round(tooltipItem.yLabel).toLocaleString();
    //             return label;
    //           } else {
    //             label +=
    //               "-$" + Math.round(tooltipItem.yLabel * -1).toLocaleString();
    //             return label;
    //           }
    //         },
    //       },
    //     },

    //     scales: {
    //       yAxes: [
    //         {
    //           ticks: {
    //             // Include a dollar sign in the ticks and add comma formatting
    //             callback: function(value) {
    //               if (value >= 0) {
    //                 return "$" + Math.round(value).toLocaleString();
    //               } else {
    //                 return "-$" + Math.round(value * -1).toLocaleString();
    //               }
    //             },

    //             fontColor: "rgb(56,56,56)",
    //           },

    //           scaleLabel: {
    //             display: true,
    //             labelString: "Difference in after-tax income",
    //             fontColor: "rgb(56,56,56)",
    //             fontStyle: "bold",
    //             fontSize: 15,
    //           },

    //           gridLines: {
    //             drawTicks: false,
    //             zeroLineColor: "rgb(56,56,56)",
    //             zeroLineWidth: 2,
    //           },
    //         },
    //       ],

    //       xAxes: [
    //         {
    //           ticks: {
    //             callback: function(value) {
    //               return "$" + Math.round(value).toLocaleString();
    //             },

    //             fontColor: "rgb(56,56,56)",

    //             maxTicksLimit: 15,
    //             min: 0,
    //             max: 300000,
    //             minRotation: 90,
    //             maxRotation: 90,
    //           },

    //           scaleLabel: {
    //             display: true,
    //             labelString: "Pre-Tax Employment Income",
    //             fontColor: "rgb(56,56,56)",
    //             fontStyle: "bold",
    //             fontSize: 15,
    //           },

    //           gridLines: {
    //             drawTicks: false,
    //             zeroLineColor: "rgb(56,56,56)",
    //             zeroLineWidth: 2,
    //           },
    //         },
    //       ],
    //     },

    //     legend: {
    //       labels: {
    //         fontColor: "rgb(56,56,56)",
    //         boxWidth: 13,
    //         padding: 10,
    //       },
    //     },

    //     title: {
    //       display: true,
    //       text:
    //         "Difference in After-Tax Income: " +
    //         this.provinceA +
    //         " vs " +
    //         this.provinceB,
    //       fontSize: 18,
    //       fontColor: "rgb(56,56,56)",
    //       padding: 2,
    //     },

    //     plugins: {
    //       datalabels: {
    //         formatter: function(value) {
    //           return "$" + Math.round(value).toLocaleString();
    //         },

    //         display: false,
    //       },
    //     },
    //   },
    // });
    // },

    // executeFunctionByName(functionName, context /*, args */) {
    //   var args = Array.prototype.slice.call(arguments, 2);
    //   var namespaces = functionName.split(".");
    //   var func = namespaces.pop();
    //   for (var i = 0; i < namespaces.length; i++) {
    //     context = context[namespaces[i]];
    //   }
    //   console.log("wookip", context[func]);
    //   return context[func].apply(context, args);
    // },

    fillTable() {
      this.mainTable = document.getElementById("mainTable");

      for (var i = 0; i < this.dataLength; i++) {
        var tableRow = document.createElement("tr");

        tableRow.setAttribute("id", "row" + (i + 1));
        this.mainTable.appendChild(tableRow);

        for (var j = 0; j < this.numberCol; j++) {
          var tableCell = document.createElement("td");
          tableCell.classList.add("column" + j);
          tableCell.setAttribute("id", "row" + (i + 1) + "col" + (j + 1));
          tableRow.appendChild(tableCell);

          if (j == 0) {
            tableCell.innerHTML = this.labelArraySorted[i];
          }

          if (j == 1) {
            if (isNaN(this.totalGrossIncomeArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" +
                Math.round(
                  this.totalGrossIncomeArraySorted[i]
                ).toLocaleString();
            }
          }

          if (j == 2) {
            if (isNaN(this.federalTaxArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" +
                Math.round(this.federalTaxArraySorted[i]).toLocaleString();
            }
          }

          if (j == 3) {
            if (isNaN(this.provincialTaxArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" +
                Math.round(this.provincialTaxArraySorted[i]).toLocaleString();
            }
          }

          if (j == 4) {
            if (isNaN(this.CPPQPPTaxArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" + Math.round(this.CPPQPPTaxArraySorted[i]).toLocaleString();
            }
          }

          if (j == 5) {
            if (isNaN(this.EITaxArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" + Math.round(this.EITaxArraySorted[i]).toLocaleString();
            }
          }

          if (j == 6) {
            if (isNaN(this.QPIPTaxArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" + Math.round(this.QPIPTaxArraySorted[i]).toLocaleString();
            }
          }

          if (j == 7) {
            if (isNaN(this.totalTaxesArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" +
                Math.round(this.totalTaxesArraySorted[i]).toLocaleString();
            }
          }

          if (j == 8) {
            if (isNaN(this.netIncomeArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                "$" + Math.round(this.netIncomeArraySorted[i]).toLocaleString();
            }
          }

          if (j == 9) {
            if (isNaN(this.avgTaxRateArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                (
                  Math.round(this.avgTaxRateArraySorted[i] * 1000) / 10
                ).toLocaleString() + "%";
            }
          }

          if (j == 10) {
            if (isNaN(this.marginalTaxRateArraySorted[i])) {
              tableCell.innerHTML = "n/a";
            } else {
              tableCell.innerHTML =
                (
                  Math.round(this.marginalTaxRateArraySorted[i] * 1000) / 10
                ).toLocaleString() + "%";
            }
          }
        }
      }
    },

    // displayMoreInfo() {
    //   if (this.moreInfoSwitch == 0) {
    //     this.moreInfoSwitch = 1;
    //     this.incomeDescriptionDiv.classList.remove("hide");
    //     this.moreInfoButton.innerHTML = '<i class="fas fa-minus-circle"></i>';
    //     //moreInfoButton.innerHTML = "&#9447";
    //   } else {
    //     this.moreInfoSwitch = 0;
    //     this.incomeDescriptionDiv.classList.add("hide");
    //     this.moreInfoButton.innerHTML = '<i class="fas fa-info-circle"></i>';
    //   }
    // },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
