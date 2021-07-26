<template>
    <v-container>
        <v-app>
            <RollView
                ref="rollView"
                @close="resumeAvailable"
                :resume-game="resumeGame"
            ></RollView>
            <v-card class="mx-auto" max-width="700">
                <v-card-text>
                    <v-row class="text-center">
                        <v-col cols="12">
                            <v-img
                                :src="
                                    'http://roll.diceapi.com/images/poorly-drawn/d6/4.png'
                                "
                                class="my-3"
                                contain
                                height="200"
                            />
                        </v-col>

                        <v-col colls="12" class="mb-4">
                            <h1 class="display-2 font-weight-bold mb-3">
                                Roll Dice Game
                            </h1>

                            <p class="subheading font-weight-bold">
                                Rules are simple
                                <br />Guess if next roll is higher or lower than
                                previous one <br />Dice is between 1 and 6
                                <br />Correct answer grants 0.1 point. You can
                                get 3 points throughout 30 rounds.
                            </p>
                        </v-col>

                        <v-col v-if="!isResume" cols="12">
                            <v-btn
                                rounded
                                color="primary"
                                dark
                                @click="play(false)"
                                ><span class="mr-2">Play</span
                                ><v-icon>play_arrow</v-icon></v-btn
                            >
                        </v-col>
                        <v-col v-else cols="12">
                            <v-col cols="12">
                                <h1 class="display-2 font-weight-bold mb-3">
                                    Reload the previous game?
                                </h1>
                            </v-col>
                            <v-col cols="12">
                                <v-btn
                                    rounded
                                    color="primary"
                                    dark
                                    @click="play(true)"
                                    ><span class="mr-2">YES</span
                                    ><v-icon>keyboard_return</v-icon></v-btn
                                ></v-col
                            >
                            <v-col cols="12">
                                <v-btn
                                    rounded
                                    color="primary"
                                    dark
                                    @click="play(false)"
                                    ><span class="mr-2">NO, Play Again</span
                                    ><v-icon>play_arrow</v-icon></v-btn
                                >
                            </v-col>
                        </v-col>
                    </v-row>
                    <p />
                </v-card-text>
            </v-card>
        </v-app>
    </v-container>
</template>

<script>
import RollView from "@/components/RollView";
export default {
    components: { RollView },
    name: "Menu",

    data: () => ({
        isResume: false,
        resumeGame: false
    }),
    // beforeMount() {
    //     if (localStorage.getItem(1)) {
    //         this.isResume = true;
    //     }
    // },
    methods: {
        play(resumeGame) {
            if (resumeGame) {
                this.resumeGame = true;
            } else {
                localStorage.clear();
                this.resumeGame = false;
            }

            localStorage.removeItem("activeDarkSite");
            localStorage.removeItem("loglevel:webpack-dev-server");
            this.$refs.rollView.open();
        },
        resumeAvailable(resumeAvailable) {
            this.isResume = resumeAvailable;
        }
    }
};
</script>
