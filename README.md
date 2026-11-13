# CodeAlpha DevOps Internship – Project 2

## Jenkins Remoting: Remote Agent Configuration and Distributed Builds

### Project Overview

This project demonstrates how to configure Jenkins Remoting to connect remote build agents to a Jenkins controller and execute jobs across different CPU architectures.

### Project Objectives

- Set up a Jenkins controller and remote agents.
- Connect remote agents using Jenkins Remoting.
- Distribute jobs using Jenkins node labels.
- Execute workloads on AMD64 and ARM64 agents.
- Explore agent isolation and security practices.

### Environment

The project was implemented using Docker containers on Ubuntu running under WSL2.

| Component | Configuration |
|---|---|
| Jenkins Controller | jenkins/jenkins:lts-jdk21 |
| AMD64 Agent 1 | linux-agent-1-new |
| AMD64 Agent 2 | linux-agent-2 |
| ARM64 Agent | arm-agent-1 |
| Java | 21 |
| Agent Connection | Inbound WebSocket |
| Container Runtime | Docker |

### Architecture

The Jenkins controller runs in a Docker container and manages three remote agents:

- `linux-agent-1-new`: AMD64 architecture (replacement agent).
- `linux-agent-2`: AMD64 architecture.
- `arm-agent-1`: ARM64 architecture.

All containers run on the same WSL2 host. The ARM64 agent uses architecture emulation on the AMD64 host; it is not a separate physical ARM machine.

### Implementation

1. Created the Jenkins controller using Docker Compose.
2. Configured persistent Jenkins storage.
3. Created permanent Jenkins nodes for AMD64 and ARM64 agents.
4. Connected the agents using inbound WebSocket connections.
5. Assigned node labels to control job execution.
6. Created Jenkins jobs to test remote execution and workload distribution.

### Test Results

The following Jenkins jobs completed successfully:

| Jenkins Job | Build | Result |
|---|---:|---|
| AMD64-Agent-1-Test | #1 | SUCCESS |
| Replacement-Agent-Test | #1 | SUCCESS |
| AMD64-Agent-2-Test | #1 | SUCCESS |
| ARM64-Agent-Test | #1 | SUCCESS |
| Remote-Agent-Distribution-Test | #4 | SUCCESS |
| Remote-Agent-Test | #1 | SUCCESS |

The console output for each successful build is available in the `logs/` directory.

### Security Practices

- Agents ran as the non-root `jenkins` user.
- Agent containers were not run in privileged mode.
- No Docker socket was mounted into the agents.
- Inbound agent connections used WebSocket.
- Node labels controlled job placement.
- Secrets and credentials must not be committed to GitHub.

**Security limitation:** All containers ran on the same WSL2 host. Container isolation is not equivalent to separate virtual machines or physical machines. This project demonstrates basic isolation practices, not production-grade isolation.

### Technologies Used

- Jenkins and Jenkins Remoting
- Docker and Docker Compose
- Ubuntu Linux (WSL2)
- Java 21
- AMD64 and ARM64 emulation
- Git and GitHub

### Learning Outcomes

I gained practical experience configuring Jenkins remote agents, establishing controller-agent connectivity, assigning jobs using node labels, and testing workloads across different CPU architectures.

I also explored container isolation and the importance of protecting Jenkins credentials and agent secrets.

### Author

CodeAlpha DevOps Internship - Project 2
