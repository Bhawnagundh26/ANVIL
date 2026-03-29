

# Oracle: applies gold patches - all tests should PASS
anvil run-evals \
  --dataset datasets/my-dataset \
  --agent oracle \
  --dockerhub-username <username> \
  --dockerhub-repo anvil-images
The oracle agent skips LLM rollouts and applies gold patches from gold_patches.json directly. All tests should pass if your harness is correct.

Prerequisites: Requires Modal and Docker Hub setup (see Setup).

Options
Flag	Default	Description
--model	—	Model ID (required for agents, optional for oracle)
--dataset	—	Dataset ID or path
--dockerhub-username	—	Docker Hub username
--dockerhub-repo	—	Docker Hub repo name
--agent	mini-swe-agent	Agent to use (mini-swe-agent or oracle)
--n-attempts	1	Attempts per task (for pass@k)
--max-parallel	30	Concurrent agent runs
--no-continue	false	Start fresh, ignore previous results
--max-wait	auto	Minutes to wait for Modal rate limits
Creating Custom Tasks
Anvil includes a task creation wizard to help you build your own evaluation datasets.

Quick Start
# 1. Initialize dataset with your repository
anvil init-dataset -d my-dataset --repo-path ./my-repo --base-image golang:1.22

# 2. Add tasks (problem statement + solution patch + tests)
anvil add-task -d my-dataset \
  --problem-file problem.md \
  --patch-file solution.diff \
  --tests-file tests.py \
  --fail-to-pass "test_feature_works,test_edge_case"

# 3. Convert to Anvil evaluation format
anvil convert-dataset -d my-dataset -u <dockerhub-username>

# 4. Publish and verify with oracle
anvil publish-images -d my-dataset -u <dockerhub-username> --repo anvil-images
anvil run-evals -d my-dataset --agent oracle -u <dockerhub-username> --dockerhub-repo anvil-images
See docs/TASK_CREATION_GUIDE.md for the complete guide including:

Task file format reference
Troubleshooting tips
Full workflow example
