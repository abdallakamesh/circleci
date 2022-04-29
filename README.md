# circleci
envirnment for circleci
# Use the latest 2.1 version of CircleCI pipeline process engine.
# See: https://circleci.com/docs/2.0/configuration-reference
version: 2.1

# Define a job to be invoked later in a workflow.
# See: https://circleci.com/docs/2.0/configuration-reference/#jobs
commands:
  print_pipeline_id :
    description: "Print the workflow ID to the console."
    steps:
      - run : "echo '{{CIRCLE_WORKFLOW_ID}}'" 
      # {{CIRCLE_WORKFLOW_ID}} is a variable that is set by CircleCI. 
jobs:
  print_command:
    docker:
      - image: circleci/node:13.8.0
    steps:
      - print_pipe_id

  save_hello_world_output:
    docker:
      - image: circleci/node:13.8.0
    steps:
      - run:
         echo "hello world this is abdalla kamesh 2" > ~/output.txt
      - persist_to_workspace:
          root: ~/
          paths:
            - output.txt

  print_output_file :
    docker:
      - image: circleci/node:13.8.0
    steps:
      - attach_workspace:
          at: ~/
      - run: cat ~/output.txt

# Invoke jobs via workflows
# See: https://circleci.com/docs/2.0/configuration-reference/#workflows
workflows:
  save_hello_world_output-workflow:
    jobs:
      - print_command

      - print_output_file:
          requires:
            - save_hello_world_output

