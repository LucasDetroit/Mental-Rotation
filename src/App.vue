<template>
  <Experiment title="mental-rotation">
    <InstructionScreen :title="'Mental Rotation'">
      Welcome to this mock up experiment to showcase the functionalities of Magpie.
      <br />
      <br />
      This is part of the course 'Introduction to Reproducible Research Practices Via Browser-Based Replication' at the Tübingen Universität.
      <br />
      <br />
      Follwoing you will find the instructions to complete it properly. Please click on the 'next' button below. 
    </InstructionScreen>

    <InstructionScreen :title="'General Instructions'">
      This is a sample instructions view.
      <br />
      <br />
      First you will go through two practice trials. The practice trial view
      uses magpie's key press trial input.
    </InstructionScreen>

    <!-- Practice: Here we create screens in a loop for every entry in keypress_choice -->
    <template v-for="(training_figure, i) of training_trials" >
      <KeypressScreen
        :fixation-time="fixation_time"
        :response-time="7500"
        :feedback-time="800"
        :key="'keypress-'  + i"
        :keys="{'f': 'same', 'j': 'different'}" >
        <template #stimulus >
          <img :src="training_figure.picture" />
          <Record :data="{angle, expected, item, trial_type, trial_number}" />
        </template>
        <template #feedback>
          <p v-if="!$magpie.measurements.hasOwnProperty('response')">Faster!</p>
          <p v-else-if="$magpie.measurements.response === 'same'">You selected 'same'</p>
          <p v-else>You selected 'different'</p>
          
        </template>
      </KeypressScreen>
    </template>
  
    <InstructionScreen :title="'Practice done'">
      You did well!
      <br />
      <br />
      Now you'll go through the actual trials in the experiment. Follow the same instructions as before.
       <br />
      <br />
      Go!
    </InstructionScreen>

<!-- Practice: Here we create screens in a loop for every entry in keypress_choice -->

    <template v-for="(main_figure, i) of main_trials" >
      <KeypressScreen
        :key="'keypress-'  + i"
        :keys="{'f': 'same', 'j': 'different'}" >
        
        <template #stimulus>
          <img :src="main_figure.picture" />
          <Record :data="{angle, expected, item}" />
        </template>
      </KeypressScreen>
    </template>


    <!--

      Comment this in, to try out interactive components like the Chat component.

      <ConnectInteractiveScreen />

      <Screen>
          <Chat :messages.sync="$magpie.measurements.messages"></Chat>
          <button @click="$magpie.saveAndNextScreen()">Next</button>
      </Screen>

      -->

    <PostTestScreen />

    <!-- While developing your experiment, using the DebugResults screen is fine,
      once you're going live, you can use the <SubmitResults> screen to automatically send your experimental data to the server. -->
    <SubmitResultsScreen />
  </Experiment>
</template> 

<script>
// Load data from csv files as javascript arrays with objects
import mr_training_trials from '../trials/mr_training_trials.csv';
import mr_main_trials from '../trials/mr_main_trials.csv';
import _ from 'lodash';

export default {
  name: 'App',
  data() {
    return {
      fixation_time: _.sample([280, 300, 320]),
      training_trials: _.shuffle(mr_training_trials),
      main_trials: _.shuffle(mr_main_trials),

      // Expose lodash.range to template above
      range: _.range
    };
  }
  
};
</script>
