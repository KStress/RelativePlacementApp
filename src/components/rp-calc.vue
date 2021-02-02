<template>
  <div style="margin: 0 auto; min-width: 1000px; max-width: 80%">
    <h1>{{ contestName }}</h1>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">Bib</th>
          <th scope="col">Contestant</th>
          <th
            scope="col"
            class="text-center pointer"
            v-for="jg in judges"
            :key="jg.id"
            :title="jg.name"
            v-on:click="editJudge(jg.id)"
          >
            J{{ judges.indexOf(jg) + 1 }}
            <i class="fa fa-pencil fa-xs"></i>
            <br />({{ getInitials(jg.name) }})
          </th>
          <th scope="col" class="text-center">
            <button
              class="btn btn-success btn-sm"
              v-on:click="addJudge"
              title="Add Judge"
            >
              <i class="fa fa-plus"></i>
            </button>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="ct in contestants" :key="ct.id">
          <td
            scope="row"
            v-if="
              selectedContestant === undefined ||
              selectedContestant.id !== ct.id
            "
          >
            {{ ct.bib }}
          </td>
          <td
            scope="row"
            v-if="
              selectedContestant !== undefined &&
              selectedContestant.id === ct.id
            "
          >
            <input
              type="text"
              class="form-control text-left"
              style="width: 60px"
              v-model="selectedContestant.bib"
            />
          </td>
          <td
            v-on:click="editContestant(ct.id)"
            v-if="
              selectedContestant === undefined ||
              selectedContestant.id !== ct.id
            "
            class="pointer"
          >
            {{ ct.name }} <i class="fa fa-pencil fa-xs"></i>
          </td>
          <td
            v-if="
              selectedContestant !== undefined &&
              selectedContestant.id === ct.id
            "
          >
            <div class="input-group">
              <input
                type="text"
                class="form-control"
                v-model="selectedContestant.name"
                style="width: 200px"
              />
              <div class="input-group-append">
                <button
                  class="btn btn-success btn-sm"
                  v-on:click="saveContestant"
                  title="Save Contestant"
                >
                  <i class="fa fa-check"></i>
                </button>
              </div>
              <div class="input-group-append">
                <button
                  class="btn btn-danger btn-sm"
                  v-on:click="cancelContestant"
                  title="Cancel Save Contestant"
                >
                  <i class="fa fa-times"></i>
                </button>
              </div>
            </div>
          </td>
          <td class="text-center" v-for="jg in judges" :key="jg.id">
            <input
              type="text"
              style="width: 60px; text-align: right"
              :value="getScore(ct.id, jg.id)"
            />
          </td>
          <td class="text-center">
            <button
              class="btn btn-danger btn-sm"
              v-on:click="deleteContestant(ct.id)"
              title="Delete Judge"
            >
              <i class="fa fa-times"></i>
            </button>
          </td>
        </tr>
        <tr
          v-if="
            selectedContestant === undefined ||
            selectedContestant.id !== nextContestantId
          "
        >
          <td scope="row">
            <button
              class="btn btn-success btn-sm"
              v-on:click="addContestant"
              title="Add Contestant"
              v-if="selectedContestant === undefined"
            >
              <i class="fa fa-plus"></i>
            </button>
          </td>
          <td></td>
          <td v-for="jg in judges" :key="jg.id" class="text-center">
            <button
              class="btn btn-danger btn-sm"
              v-on:click="deleteJudge(jg.id)"
              title="Delete Judge"
            >
              <i class="fa fa-times"></i>
            </button>
          </td>
          <td></td>
        </tr>
      </tbody>
    </table>
    <JudgeDetail
      :judge="selectedJudge"
      @save="saveJudge"
      @cancel="cancelJudge"
      v-if="selectedJudge"
    />
  </div>
</template>

<script>
import JudgeDetail from "@/components/judge-detail";

export default {
  name: "RPCalc",
  data() {
    return {
      contestName: "Contest Display Name",
      // selectedContestant: {
      //   id: 1,
      //   bib: 759,
      //   name: "Kelly Strouse / Denise Strouse",
      // },
      selectedContestant: undefined,
      selectedJudge: undefined,
      contestants: [
        {
          id: 1,
          bib: 759,
          name: "Kelly Strouse / Denise Strouse",
        },
        {
          id: 2,
          bib: 418,
          name: "Mark Thompson / Brigette Thompson",
        },
        {
          id: 3,
          bib: 834,
          name: "Gregg Thrash / Laura Thrash",
        },
        {
          id: 4,
          bib: 265,
          name: "Bobby Pritchard / Carla Romine",
        },
      ],
      judges: [
        {
          id: 1,
          name: "Bob Wheatley",
        },
        {
          id: 2,
          name: "Carmen Goodman",
        },
        {
          id: 3,
          name: "Beth Emerson",
        },
      ],
      scores: [
        {
          id: 1,
          jId: 1,
          cId: 1,
          scoreRank: 1,
        },
        {
          id: 2,
          jId: 1,
          cId: 2,
          scoreRank: 2,
        },
        {
          id: 3,
          jId: 1,
          cId: 3,
          scoreRank: 3,
        },
        {
          id: 4,
          jId: 1,
          cId: 4,
          scoreRank: 4,
        },
        {
          id: 5,
          jId: 2,
          cId: 1,
          scoreRank: 4,
        },
        {
          id: 6,
          jId: 2,
          cId: 2,
          scoreRank: 3,
        },
        {
          id: 7,
          jId: 2,
          cId: 3,
          scoreRank: 1,
        },
        {
          id: 8,
          jId: 2,
          cId: 4,
          scoreRank: 2,
        },
      ],
      nextJudgeId: -1,
      nextContestantId: -1,
    };
  },
  components: {
    JudgeDetail,
  },
  methods: {
    //Contestants
    addContestant: function () {
      this.selectedContestant = {
        id: this.nextContestantId,
        bib: 0,
        name: "",
      };
      this.contestants.push(this.selectedContestant);
      this.nextContestantId--;
    },
    cancelContestant: function () {
      if (this.selectedContestant.name.trim().length === 0) {
        if (this.selectedContestant.id < 0) {
          let deleteIdx = this.contestants.findIndex(
            (item) => item.id === this.selectedContestant.id
          );
          this.contestants.splice(deleteIdx, 1);
        }
        this.selectedContestant = undefined;
        return;
      }

      this.selectedContestant = undefined;
    },
    deleteContestant: function (ctId) {
      let deleteIdx = this.contestants.findIndex((item) => item.id === ctId);
      this.contestants.splice(deleteIdx, 1);
    },
    editContestant: function (ctId) {
      this.selectedContestant = {
        ...this.contestants.filter((item) => item.id === ctId)[0],
      };
    },
    saveContestant: function () {
      let matchIdx = this.contestants.findIndex(
        (item) => item.id === this.selectedContestant.id
      );
      this.contestants[matchIdx] = this.selectedContestant;
      this.selectedContestant = undefined;
    },
    //Judges
    addJudge: function () {
      this.selectedJudge = {
        id: this.nextJudgeId,
        name: "",
      };
    },
    cancelJudge: function () {
      this.selectedJudge = undefined;
    },
    deleteJudge: function (jgId) {
      let deleteIdx = this.judges.findIndex((item) => item.id === jgId);
      this.judges.splice(deleteIdx, 1);
    },
    editJudge: function (jgId) {
      this.selectedJudge = {
        ...this.judges.filter((item) => item.id === jgId)[0],
      };
    },
    saveJudge: function (judge) {
      if (judge.name.trim().length === 0) {
        this.selectedJudge = undefined;
        return;
      }

      if (judge.id > this.nextJudgeId) {
        let matchIdx = this.judges.findIndex((item) => item.id === judge.id);
        this.judges[matchIdx] = { ...judge };
      } else {
        this.judges.push({
          id: this.nextJudgeId,
          name: judge.name,
        });
        this.nextJudgeId--;
      }
      this.selectedJudge = undefined;
    },
    //Scores
    getScore: function (ctId, jgId) {
      let score = this.scores.filter(
        (item) => item.cId === ctId && item.jId === jgId
      );
      return score.length > 0 ? score[0].scoreRank : "";
    },
    //Misc
    getInitials: function (fullName) {
      return fullName
        .split(" ")
        .map((n) => n[0])
        .join("");
    },
  },
  computed: {
    isNewJudge: function () {
      return this.selectedJudge.id < 0;
    },
  },
};
</script>
