# px4-github-workflows

Reusable GitHub Actions workflows for PX4 Autopilot and ROS 2 integration testing.

## Overview

This repository provides CI/CD workflows and testing utilities for projects integrating PX4 Autopilot with ROS 2. The workflows are designed to be modular and can be inherited by other repositories as a git submodule.

## Contents

- **`.github/workflows/px4_ros_ci.yml`** - Main CI workflow for PX4+ROS 2 integration
- **`test_workflow.sh`** - Local testing script using `act` to run workflows locally

## Workflow Features

The `px4_ros_ci.yml` workflow provides:

1. **Automated Environment Setup**
   - ROS 2 Humble
   - Gazebo Garden simulator
   - PX4 Autopilot (release/1.14)
   - Micro XRCE-DDS Agent

2. **Build Process**
   - PX4 SITL compilation
   - ROS 2 workspace build (px4_msgs, px4_ros_com)

3. **Integration Testing**
   - Starts DDS agent and PX4 simulation
   - Verifies ROS 2 topic communication
   - Validates sensor data flow

4. **Artifacts & Logging**
   - Uploads logs on failure
   - Detailed error reporting

## Usage in Other Repositories

### As a Submodule

```bash
# Add as a submodule
git submodule add https://github.com/derickcoder44/px4-github-workflows.git

# Use the workflow in your repo
# Copy or symlink .github/workflows/px4_ros_ci.yml to your .github/workflows/
```

### As a Reusable Workflow

You can also reference this workflow directly:

```yaml
# In your .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  px4-integration:
    uses: derickcoder44/px4-github-workflows/.github/workflows/px4_ros_ci.yml@main
```

## Local Testing

Test workflows locally before pushing:

```bash
# Install act (GitHub Actions local runner)
brew install act  # macOS
# or
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash  # Linux

# Run the workflow locally
./test_workflow.sh
```

## Customization

The workflow can be customized through environment variables:
- `PX4_BRANCH` - PX4 Autopilot branch (default: release/1.14)
- `ROS_DISTRO` - ROS distribution (default: humble)
- `HEADLESS` - Run simulation headless (default: 1)

## Requirements

- Ubuntu 22.04 (or compatible runner)
- Docker (for local testing with act)

## Integration

This module is designed to work with:
- [docker-px4-core](https://github.com/derickcoder44/docker-px4-core) - Core PX4 Docker scripts
- [code-checker](https://github.com/derickcoder44/code-checker) - Code quality checks

## License

See LICENSE file for details.
