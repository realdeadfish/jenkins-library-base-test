@Library('jenkins-library-base@master')
import com.ibm.oip.jenkins.*;
import com.ibm.oip.jenkins.steps.*;
import com.ibm.oip.jenkins.steps.java.*;

Pipeline master = new PipelineBuilder().forMaster().withSteps([
Common.PARALLEL(
  new Step(){
    void doStep(BuildContext buildContext) {
        buildContext.changeStage("foo") {
           sh "echo 'step 1'"
        }
   }
  },
     new Step(){
    void doStep(BuildContext buildContext) {
        buildContext.changeStage("bar") {
           sh "echo 'step 2-1'"
        }
        buildContext.changeStage("baz") {
           sh "echo 'step 2-2'"
           checkout([
                              $class           : 'GitSCM',
                              branches         : "master",
                              extensions       : [[$class: 'LocalBranch', localBranch: "master"], [$class: 'CloneOption', depth: 0, noTags: false, reference: '', shallow: false]]
                      ])
        }
   }
   },
   new Step(){
    void doStep(BuildContext buildContext) {
        buildContext.getScriptEngine().sh "echo 'step 3'"
   }
   String name() { return "Step 3" }
  }
)]).build();

new PipelineRunner()
  .withScriptEngine(this)
  .withBranch("master")
  .withPossiblePipelines(master)
  .run();

