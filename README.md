# PolyScope X No Install SDK (Codespace)

This repository contains the PolyScope X SDK and URSim artefacts.
## Intention of this repository

This repository is designed to enable users to quickly and easily develop, build and test URCaps for PolyScope X using [GitHub Codespaces](https://docs.github.com/en/codespaces/overview) for developers who prefer online development over local development.

You can find documentation, references and tutorials for the entire URCap ecosystem and developing URCaps in general in the official [PolyScope X SDK documentation](https://docs.universal-robots.com/PolyScopeX_SDK_Documentation/) website as well 

If you prefer local development you can refer to the [setup section of the official PolyScope X SDK documentation](https://docs.universal-robots.com/PolyScopeX_SDK_Documentation/build/SDK-v0.22/Setup.html).

### SDK Versioning
All versions of the SDK and URSim available here can also be found on the primary release site in the PolyScope X Beta
Project.

The release provided here is offered "AS IS" for the sake of rapid development, if you would like to be more involved and provide feedback on recent releases,
please sign up for the [PolyScope X Beta Project](https://ur.centercode.com/EarlyAccessTesterPSX).
You can also connect with other developers by joining
the [Universal Robots Discord server](https://discord.com/invite/sEjRgEf6fp).

### Note
You can install URCaps from local storage via the system manager in URSim.
However, due to codespace transfer restrictions, there is a limit to the size of URCaps that can be installed this way.
When building and installing URCaps from codespace there are no size restrictions. For more details see "Build and
Deploy URCap" section below.

### Table of contents

- [PolyScope X and URCaps at a glance](#polyscope-x-and-urcaps-at-a-glance)
  - [What is PolyScope X?](#what-is-polyscope-x)
  - [What is a URCap?](#what-is-a-urcap)
- [URCap architecture and contributions](#urcap-architecture-and-contributions)
  - [Frontend contribution types](#frontend-contribution-types)
  - [Backend container contributions](#backend-container-contributions)
- [Develop a URCap in GitHub Codespaces](#develop-a-urcap-in-github-codespaces)
  - [1. Start a Codespace from the repository](#1-start-a-codespace-from-the-repository)
  - [2. Start URSim and forward its port](#2-start-ursim-and-forward-its-port)
  - [3. Explore the SDK](#3-explore-the-sdk)
  - [4. Create a URCap](#4-create-a-urcap)
  - [5. Build and deploy the URCap](#5-build-and-deploy-the-urcap)
  - [6. Stop or delete the Codespace](#6-stop-or-delete-the-codespace)


## PolyScope X and URCaps at a glance

### What is PolyScope X?

PolyScope X is the software environment used to configure, program, and operate a Universal Robots robot. It provides the user interfaces used by robot programmers and operators, manages robot programs and applications, loads installed URCaps, and starts and monitors robot programs.

For a URCap developer, PolyScope X can be understood as the platform that hosts URCap functionality and connects it with the robot controller, the user interface, and supported external systems.

PolyScope X and its URCaps use technologies such as Linux, Docker, Angular, React.js, and ROS 2.

### What is a URCap?

A URCap is an installable software extension for PolyScope X, similar to an app installed on a smartphone. A URCap can extend how users configure, program, and operate a Universal Robots robot.

A URCap can, for example:

- Add configuration or programming screens.
- Provide custom program nodes or operator controls.
- Display production information or device status.
- Communicate with the robot controller or external hardware.
- Run background services or custom computations.
- Integrate supported devices such as grippers, sensors, or other equipment.

A URCap is the complete installable package. The individual features within the package are called **contributions**.

## URCap architecture and contributions

URCap functionality is generally divided into two groups:

- **Frontend contributions** extend the PolyScope X user interface and programming environment.
- **Backend container contributions** run services or other software inside Docker containers.

A simple URCap may contain only one frontend or backend contribution. A more advanced URCap may combine several frontend contributions with one or more backend containers.

For example, a gripper URCap could contain:

```text
Gripper URCap
├── Application contribution     # Configure connection and gripper settings
├── Program Node contribution    # Add open/close commands to a robot program
└── Container contribution       # Communicate with the physical gripper
```

### Frontend contribution types

Frontend contributions are hosted by PolyScope X in the appropriate part of its interface. They can present information, accept and validate user input, store configuration, generate URScript, communicate with a backend contribution, and use supported PolyScope X APIs.

| Contribution type | Typical purpose |
| --- | --- |
| **Application** | Installation-wide or device configuration, such as network settings, fixed parameters, function definitions, and global-variable initialization. |
| **Program Node** | Adds an action or process step to the robot program. It can generate URScript and is well suited for robot process and flow control. |
| **Sidebar** | Provides quick actions or status information from a panel available across manual-mode screens. It cannot contribute URScript and is unavailable in remote control mode. |
| **Operator Screen** | Provides a wide production-facing screen for operators. It can display information and allow value changes where permitted, but it cannot contribute URScript. |
| **Smart Skill** | Provides a configurable action. It can support teaching, one-off manual actions, and insertion of behavior into a robot program. |

### Backend container contributions

A backend contribution runs in a Docker container and does not automatically appear in the PolyScope X user interface. It is useful when a URCap must:

- Run background or long-running tasks.
- Communicate continuously with external hardware or services.
- Perform custom or computationally intensive work.
- Reuse existing Python, C++, Java, ROS 2, or other software.
- Expose REST, WebSocket, XML-RPC, or similar services.
- Communicate with supported robot controller interfaces.
- Provide services to frontend contributions.

Use a frontend contribution when the functionality belongs in the PolyScope X interface, supported frontend APIs, or generated URScript. Use a backend container when work must continue independently of the interface or requires a separate runtime, library, service, or communication process. Use both when users must configure or monitor functionality in PolyScope X while a background service performs the actual communication or computation.

# Develop a URCap in GitHub Codespaces
This tutorial will explain the development workflow using [GitHub Codespaces](https://docs.github.com/en/codespaces/overview), for the complete documentation or local development workflow refer to the official [PolyScope X SDK documentation](https://docs.universal-robots.com/PolyScopeX_SDK_Documentation/)

## 1. Start a Codespace from the repository

> **Prerequisite:** You need a GitHub account and must be signed in to use Codespaces.

1. Open the **Code** dropdown on the repository page.
2. Select the **Codespaces** tab.
3. Create a Codespace on `master` or another branch.
4. Wait for the browser-based Visual Studio Code environment to start and build the development container. Setup is complete after the `postCreateCommand` and `postStartCommand` have run successfully.

You can reopen your cloud workspaces from [GitHub Codespaces](https://github.com/codespaces).

## 2. Start URSim and forward its port

After the Codespace has finished starting, open a terminal and run:

```bash
./run-simulator --port <PORT_NUMBER>
```

If you omit the port, URSim uses port `80`.

To access the URSim interface:

1. Open the **PORTS** tab next to the terminal.
2. Add the port used by `run-simulator` in the **Port** column and press **Enter**.
3. Open the generated URL in the **Forwarded Address** column.

> **Note:** A Codespace has a default idle timeout of 30 minutes. After it stops, you may need to restart it and forward the port again. See [Setting your timeout period for GitHub Codespaces](https://docs.github.com/en/codespaces/setting-your-user-preferences/setting-your-timeout-period-for-github-codespaces).

## 3. Explore the SDK

Open a new terminal. The main SDK contents are:

- `urcap-generator/`: Generator used to create URCap contributions.
- `samples/`: Example URCaps. Build a sample with `npm install && npm run build`.
- `newurcap.sh`: Creates a URCap contribution in the current working directory using the generator.
- `run-simulator`: Starts URSim. This is a linked file; the full URSim directory is available at `/`.

### Working with samples

Treat the `samples/` folder as read-only. Rebuilding the Codespace container removes and recreates this folder.

To preserve changes, copy the sample to a location outside `samples/` before editing it, then commit the copied project to your own Git repository.

## 4. Create a URCap

Run the generator script from the directory where you want to create the project:

```bash
./newurcap.sh
```

After the generator finishes, it creates a project folder named after the URCap ID.

## 5. Build and deploy the URCap

Open the generated URCap folder and run the following commands.

Install the Node.js dependencies:

```bash
npm install
```

Build the URCap package in the `target` directory:

```bash
npm run build
```

With URSim running and accessible, install the URCap:

```bash
npm run install-urcap
```

The URCap should now be available in URSim.

> **Installation note:** URCaps can also be installed from local storage through the URSim System Manager. Codespace transfer restrictions may limit the package size when using that method. Building and installing directly from the Codespace does not have the same transfer-size restriction.

For further development guidance, see the [PolyScope X SDK official documentation](https://docs.universal-robots.com/).

## 6. Stop or delete the Codespace

From the repository's **Code** dropdown, open the menu for the Codespace and choose one of the following actions:

- **Stop codespace**: Stops all running processes while preserving the Codespace so it can be started again.
- **Delete**: Permanently removes the Codespace instance.

For more information about stopping, rebuilding, reopening, and deleting Codespaces, see [Understanding the Codespace lifecycle](https://docs.github.com/en/codespaces/getting-started/understanding-the-codespace-lifecycle).
