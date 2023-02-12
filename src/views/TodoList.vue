<template>
  <v-container>
    <v-row justify="center" class="mt-5">
      <v-alert
        border="right"
        v-if="alertCheck"
        type="info"
        width="400"
        color="primary"
        >개발중이지롱</v-alert
      >
    </v-row>
    <v-row justify="center">
      <v-col cols="12" sm="7" md="7" lg="5">
        <v-row>
          <v-col cols="8">
            <v-text-field
              v-model="newTask"
              label="필요한거 추가하기~"
              solo
              @keydown.enter="create"
            >
              <template v-slot:append>
                <v-fade-transition>
                  <v-icon v-if="newTask" @click="create">
                    mdi-plus-circle
                  </v-icon>
                </v-fade-transition>
              </template>
            </v-text-field>
          </v-col>

          <v-col cols="4" class="pl-0">
            <v-btn
              color="primary"
              height="48"
              block
              @click="alertCheck = !alertCheck"
              class="black--text"
            >
              안한거 체크하기
            </v-btn>
          </v-col>
        </v-row>

        <v-row class="my-1" align="center">
          <strong class="mx-4 info--text">
            Remaining: {{ remainingTasks }}
          </strong>

          <v-divider vertical></v-divider>

          <strong class="mx-4 success--text text--darken-1">
            Completed: {{ tmpLength + completedTasks }}
          </strong>
        </v-row>

        <v-divider class="mb-4"></v-divider>

        <v-card v-if="tasks.length > 0">
          <v-scroll-x-transition class="py-0" group>
            <v-list-item v-for="(task, i) in tasks" :key="`${i}-${task.text}`">
              <v-list-item-action>
                <v-checkbox
                  v-model="task.done"
                  :color="(task.done && 'grey') || 'black'"
                  class="todo_checkbox"
                >
                  <template v-slot:label>
                    <div
                      :class="(task.done && 'grey--text') || 'black--text'"
                      class="ml-4"
                      v-text="task.text"
                    ></div>
                  </template>
                </v-checkbox>
              </v-list-item-action>

              <v-spacer></v-spacer>

              <v-scroll-x-transition>
                <v-icon v-if="task.done" color="success"> mdi-check </v-icon>
              </v-scroll-x-transition>
            </v-list-item>
          </v-scroll-x-transition>
        </v-card>
      </v-col>
    </v-row>

    <v-row justify="center" align="center">
      <v-col cols="12" sm="7" md="7" lg="5">
        <!-- 메이플 월드 -->
        <v-card class="mb-3 pa-3" elevation="8">
          <v-item-group multiple>
            <v-row>
              <div
                v-for="mapleWorld in mapleWorlds"
                :key="mapleWorld.id"
                class="quest_div"
              >
                <v-item v-slot="{ active, toggle }">
                  <v-card
                    :color="active ? '#ff7c24' : ''"
                    @click="toggle(), active ? ups() : downs()"
                    elevation="0"
                    align="center"
                  >
                    <v-card-text class="ma-0 pa-0">
                      <v-slide-y-transition>
                        <div v-if="active" class="clear_div">
                          <span class="clear_span">
                            {{ mapleWorld.title }}
                          </span>
                        </div>
                        <div v-else>
                          <v-img class="mt-1" width="60" />
                        </div>
                      </v-slide-y-transition>
                    </v-card-text>
                  </v-card>
                </v-item>
              </div>
            </v-row>
          </v-item-group>
        </v-card>

        <!-- 아케인리버 -->
        <v-card class="mb-3 pa-3" elevation="8">
          <!-- 일간 -->
          <v-item-group multiple class="mb-2">
            <v-row>
              <div
                v-for="arcaneRiverD in arcaneRiverDaily"
                :key="arcaneRiverD.id"
                class="quest_div"
              >
                <v-item v-slot="{ active, toggle }">
                  <v-card
                    :color="active ? '#9ce1f4' : ''"
                    @click="toggle(), active ? ups() : downs()"
                    elevation="0"
                    align="center"
                  >
                    <v-card-text class="ma-0 pa-0">
                      <v-scroll-x-transition>
                        <div v-if="active" class="clear_div">
                          <span class="clear_span">
                            {{ arcaneRiverD.title }}
                          </span>
                        </div>
                        <div v-else>
                          <v-img class="shrink mt-1" width="60" />
                        </div>
                      </v-scroll-x-transition>
                    </v-card-text>
                  </v-card>
                </v-item>
              </div>
            </v-row>
          </v-item-group>

          <!-- 주간 -->
          <v-item-group multiple>
            <v-row>
              <div v-for="n in arcaneRiverWeekly" :key="n.id" class="quest_div">
                <v-item v-slot="{ active, toggle }">
                  <v-card
                    :color="active ? '#9ce1f4' : ''"
                    @click="toggle(), active ? ups() : downs()"
                    elevation="0"
                    align="center"
                  >
                    <v-card-text class="ma-0 pa-0">
                      <v-scroll-x-transition>
                        <div v-if="active" class="clear_div">
                          <span class="clear_span">
                            {{ n.title }}
                          </span>
                        </div>
                        <div v-else>
                          <v-img
                            class="shrink mt-1"
                            width="60"
                            max-height="60"
                          />
                        </div>
                      </v-scroll-x-transition>
                    </v-card-text>
                  </v-card>
                </v-item>
              </div>
            </v-row>
          </v-item-group>
        </v-card>

        <!-- 테네브리스 -->
        <v-card class="mb-3 pa-3" elevation="8">
          <v-item-group multiple>
            <v-row>
              <div v-for="n in tenebris" :key="n.id" class="quest_div">
                <v-item v-slot="{ active, toggle }">
                  <v-card
                    :color="active ? '#8e6da1' : ''"
                    @click="toggle(), active ? ups() : downs()"
                    elevation="0"
                    align="center"
                  >
                    <v-card-text class="ma-0 pa-0">
                      <v-scroll-x-transition>
                        <div v-if="active" class="clear_div">
                          <span class="clear_span">
                            {{ n.title }}
                          </span>
                        </div>
                        <div v-else>
                          <span class="clear_span">
                            {{ n.title }}
                          </span>
                        </div>
                      </v-scroll-x-transition>
                    </v-card-text>
                  </v-card>
                </v-item>
              </div>
            </v-row>
          </v-item-group>
        </v-card>

        <!-- 그란디스 -->
        <v-card class="mb-3 pa-3" elevation="8">
          <v-item-group multiple>
            <v-row>
              <div v-for="n in grandis" :key="n.id" class="quest_div">
                <v-item v-slot="{ active, toggle }">
                  <v-card
                    :color="active ? 'my_color' : ''"
                    @click="toggle(), active ? ups() : downs()"
                    elevation="0"
                    align="center"
                  >
                    <v-card-text class="ma-0 pa-0">
                      <v-scroll-x-transition>
                        <div v-if="active" class="clear_div">
                          <span class="clear_span">
                            {{ n.title }}
                          </span>
                        </div>
                        <div v-else>
                          <span class="clear_span">
                            {{ n.title }}
                          </span>
                        </div>
                      </v-scroll-x-transition>
                    </v-card-text>
                  </v-card>
                </v-item>
              </div>
            </v-row>
          </v-item-group>
        </v-card>
      </v-col>
    </v-row>

    <v-row justify="center" align="center">
      <v-col cols="12" sm="7" md="7" lg="5">
        <v-row>
          <v-col
            ><v-btn
              color="primary"
              height="48"
              block
              @click="붉늑++"
              class="black--text missions-div"
            >
              <span>붉은 늑대</span>
              <br />
              <span> {{ 붉늑 }} </span>
            </v-btn></v-col
          >
          <v-col
            ><v-btn
              color="primary"
              height="48"
              block
              @click="에스페시아++"
              class="black--text missions-div"
            >
              <span>에스페시아</span>
              <br />
              <span> {{ 에스페시아 }} </span>
            </v-btn></v-col
          >
          <v-col
            ><v-btn
              color="primary"
              height="48"
              block
              @click="플토프리토++"
              class="black--text missions-div"
            >
              <span>플토, 프리토</span>
              <br />
              <span> {{ 플토프리토 }} </span>
            </v-btn></v-col
          >
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  name: "TodoList",
  data: () => ({
    붉늑: 0,
    에스페시아: 0,
    플토프리토: 0,
    alertCheck: false,
    quests: 21,
    tmpLength: 0,
    tasks: [
      {
        done: false,
        text: "우르스",
      },
      {
        done: false,
        text: "길드 출석",
      },
      {
        done: false,
        text: "길드 퀘스트",
      },
      {
        done: false,
        text: "수로",
      },
      {
        done: false,
        text: "몬스터파크",
      },
      {
        done: false,
        text: "익몬파",
      },
    ],
    newTask: null,
    mapleWorlds: [
      {
        id: "m_w_1",
        title: "크리\n티아스",
      },
      {
        id: "m_w_2",
        title: "헤이븐",
      },
      {
        id: "m_w_3",
        title: "야영지",
      },
    ],
    arcaneRiverDaily: [
      {
        id: "a_d_1",
        title: "여로",
      },
      {
        id: "a_d_2",
        title: "츄츄",
      },
      {
        id: "a_d_3",
        title: "레헬른",
      },
      {
        id: "a_d_4",
        title: "아르카나",
      },
      {
        id: "a_d_5",
        title: "모라스",
      },
      {
        id: "a_d_6",
        title: "에스페라",
      },
    ],
    arcaneRiverWeekly: [
      {
        id: "a_w_1",
        title: "에르다",
      },
      {
        id: "a_w_2",
        title: "무토",
      },
      {
        id: "a_w_3",
        title: "미드나잇",
      },
      {
        id: "a_w_4",
        title: "스피릿\t세이버",
      },
      {
        id: "a_w_5",
        title: "모라스주간",
      },
      {
        id: "a_w_6",
        title: "레이저빔",
      },
    ],
    tenebris: [
      {
        id: "t_d_1",
        title: "문브릿지",
      },
      {
        id: "t_d_2",
        title: "미궁",
      },
      {
        id: "t_d_3",
        title: "리멘",
      },
    ],
    grandis: [
      {
        id: "g_d_1",
        title: "세르니움",
      },
      {
        id: "g_d_2",
        title: "호텔",
      },
      {
        id: "g_d_3",
        title: "오디움",
      },
    ],
  }),
  methods: {
    clear() {
      console.log(this.id);
    },
    create() {
      this.tasks.push({
        done: false,
        text: this.newTask,
      });

      this.newTask = null;
    },
    ups() {
      this.quests += 1;
      this.tmpLength -= 1;
    },
    downs() {
      this.quests -= 1;
      this.tmpLength += 1;
    },
  },
  computed: {
    completedTasks() {
      return this.tasks.filter((task) => task.done).length;
    },
    remainingTasks() {
      return this.tasks.length + this.quests - this.completedTasks;
    },
  },
  components: {},
});
</script>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}
</style>
