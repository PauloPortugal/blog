---
title: Automating Manjaro Setup with Ansible
date: 2025-12-12 00:44:55 +0000
draft: false
description: >-
  An Ansible-based automation framework that streamlines post-installation configuration of Manjaro Linux.
categories:
  - DevOps
  - Linux
tags:
  - ansible
  - manjaro
  - automation
  - infrastructure-as-code
  - linux
  - devops
pin: true
image:
  path: ansible-manjaro.png
  alt: Manjaro Linux and Ansible automation - streamlining system configuration with infrastructure as code
---

## Overview

Setting up a fresh Linux installation can be a time-consuming and repetitive process. Every time you install Manjaro (or any Linux distribution), you face the same tedious tasks: installing your favorite applications, configuring your development environment, setting up tools, and tweaking system settings to match your preferences.

Enter [Manjaro Playbook](https://github.com/PauloPortugal/manjaro-playbook) - an Ansible-based automation framework that transforms this multi-hour ordeal into a single command execution. This project represents the "infrastructure as code" philosophy applied to personal workstation configuration.

## The Problem It Solves

After installing a fresh copy of Manjaro Linux with GNOME 3, you typically need to:

- Install dozens of packages from official repositories
- Configure AUR (Arch User Repository) packages
- Set up development environments (Docker, Kubernetes, multiple language runtimes)
- Install and configure productivity tools
- Configure system services like firewalls and printers
- Customize GNOME settings
- Set up communication tools

Doing this manually is error-prone, inconsistent, and wastes valuable time. Even worse, you can't easily replicate your setup across multiple machines or recover quickly from system failures.

## Architecture & Design

### Modular Role-Based Structure

The playbook follows Ansible best practices with a modular, role-based architecture. Each role represents a logical component of your system configuration:

```text
manjaro-playbook/
├── roles/
│   ├── base/              # System foundation packages
│   ├── users/             # User account configuration
│   ├── browsers/          # Web browsers
│   ├── dev-tools/         # Development stack
│   ├── editors/           # Code editors & IDEs
│   ├── virtualization/    # Vagrant, VirtualBox
│   ├── comms/             # Communication tools
│   ├── cloud-tools/       # Cloud platform CLIs
│   ├── multimedia/        # Media applications
│   ├── audio-tools/       # Audio production
│   ├── printers/          # Printing services
│   ├── clamav/            # Antivirus
│   └── ...
```

This modular design provides several benefits:

- **Selective Installation**: Install only what you need using tags
- **Maintainability**: Each role is self-contained and easy to update
- **Reusability**: Roles can be shared across different playbooks
- **Clarity**: Clear separation of concerns makes the codebase understandable

### Package Management Strategy

One of Manjaro Playbook's strengths is how it handles multiple package sources:

**Official Repositories (Pacman)**: Core system packages and widely-used applications are installed via Manjaro's official repositories using the `pacman` module.

**AUR Packages**: The Arch User Repository provides access to thousands of community-maintained packages. The playbook uses `pamac` (Manjaro's package manager) to automate AUR package installation, which is typically a manual process.

**Custom Scripts**: For packages requiring special handling or those not available through standard channels, the playbook includes custom shell scripts.

## Key Features

### Development Environment Stack

The `dev-tools` role is particularly comprehensive, installing a complete modern development stack:

- **Containerization**: Docker and Kubernetes (kubectl, minikube)
- **Languages & Runtimes**: Java (OpenJDK), Node.js, Go, Scala
- **Build Tools**: Maven, Gradle, npm
- **Version Control**: Git with configuration
- **Cloud Tools**: AWS CLI, Azure CLI, Google Cloud SDK

All configured and ready to use after a single playbook run.

### Testing Infrastructure

One of the most impressive aspects of this project is its testing infrastructure. Before deploying changes to your production system, you can:

**Vagrant Testing**: The playbook includes a complete Vagrant configuration that spins up a VirtualBox VM running Manjaro, applies the playbook, and lets you verify everything works correctly in isolation.

```bash
# Test the entire playbook in a VM
vagrant up
```

This is a game-changer for safety - you never have to worry about a misconfigured playbook breaking your main system.

### Code Quality Automation

The project implements several quality assurance mechanisms:

- **Pre-commit Hooks**: Automatically validate code before commits
- **YAML Linting**: Ensures all YAML files follow best practices
- **Ansible Linting**: Checks playbooks against Ansible best practices
- **CI/CD Pipeline**: GitHub Actions automatically test changes

This level of quality control ensures reliability and maintainability.

## Getting Started

### Prerequisites

After a fresh Manjaro installation, you need:

```bash
# Update system mirrors and packages
sudo pacman-mirrors --fasttrack
sudo pacman -Syu

# Install Git and Ansible
sudo pacman -S git ansible python-pip
```

### Installation

Clone the repository and run the setup script:

```bash
# Clone via SSH (requires GitHub SSH keys configured)
git clone git@github.com:PauloPortugal/manjaro-playbook.git
cd manjaro-playbook

# Install dependencies
./setup.sh
```

### Running the Playbook

The playbook requires three variables for execution:

```bash
ansible-playbook main.yml \
  --extra-vars "username=your_username" \
  --extra-vars "git_user_name='Your Name'" \
  --extra-vars "git_user_email=your@email.com"
```

### Selective Installation

You can use Ansible tags to install only specific components:

```bash
# Install only browsers
ansible-playbook main.yml --tags browsers

# Install development tools and editors
ansible-playbook main.yml --tags dev-tools,editors

# Install everything except virtualization
ansible-playbook main.yml --skip-tags virtualization
```

## Real-World Use Cases

### Fresh Installation Recovery

**Scenario**: Your SSD fails, and you need to set up a new system quickly.

**Solution**: Install Manjaro, clone the playbook, run it. Within 30-60 minutes (depending on internet speed), you have your complete development environment back, configured exactly as it was.

### Multi-Machine Consistency

**Scenario**: You work on multiple machines (desktop, laptop, work computer) and want identical environments.

**Solution**: Run the same playbook on all machines. Every system will have the same tools, configurations, and settings.

### Onboarding New Team Members

**Scenario**: Your team uses Manjaro, and new developers need standardized setups.

**Solution**: Fork the playbook, customize it for your team's needs, and provide it as an onboarding tool. New team members can be productive on day one.

### Configuration Experimentation

**Scenario**: You want to try new tools or configurations without risking your main system.

**Solution**: Use the Vagrant testing infrastructure to experiment in a VM first, then apply successful changes to your production system.

## Technical Highlights

### Idempotency

The playbook is designed to be idempotent - you can run it multiple times safely. Ansible's declarative approach ensures that:

- Packages already installed won't be reinstalled
- Existing configurations won't be overwritten unnecessarily
- The system converges to the desired state regardless of starting condition

### Error Handling & Debug Modes

The playbook includes configurable verbosity levels:

```bash
# Standard execution
ansible-playbook main.yml

# Verbose output for troubleshooting
ansible-playbook main.yml -v

# Maximum verbosity for debugging
ansible-playbook main.yml -vvv
```

### Version Control & Release Management

The project follows professional software engineering practices:

- **Semantic Versioning**: Clear version numbers indicating compatibility
- **Change Tracking**: Over 250 commits documenting evolution
- **Release Notes**: Detailed changelogs for each version
- **GitHub Actions**: Automated CI/CD pipeline

## Lessons Learned & Best Practices

After maintaining this project across multiple releases, several best practices emerge:

1. **Start Small**: Begin with a minimal playbook and gradually add roles as needed
2. **Test First**: Always test in Vagrant before applying to production
3. **Document Everything**: Use clear variable names and include comments explaining non-obvious choices
4. **Version Lock Critical Dependencies**: Pin important package versions to avoid breakage
5. **Separate Secrets**: Never commit sensitive data; use Ansible Vault for credentials
6. **Keep Roles Focused**: Each role should do one thing well
7. **Tag Generously**: Liberal use of tags provides flexibility in execution

## Future Enhancements

While the current playbook is mature and functional, there's always room for improvement:

- **Configuration Management**: Externalize more configurations to variables files
- **Dotfiles Integration**: Automated management of shell configurations and dotfiles
- **Backup Integration**: Automated backup configuration and restoration
- **Profile Variants**: Different profiles for different use cases (development, gaming, content creation)
- **Distribution Support**: Extend support to other Arch-based distributions

## Conclusion

The Manjaro Playbook represents automation done right - it solves a real problem, follows best practices, includes comprehensive testing, and maintains high code quality. Whether you're setting up a new system, maintaining multiple machines, or just want to document your perfect development environment, infrastructure-as-code approaches like this playbook are invaluable.

The project demonstrates that personal automation projects can achieve the same professional standards as enterprise infrastructure code. By treating your workstation configuration as code, you gain reproducibility, consistency, and peace of mind knowing you can rebuild your environment at any time.

If you're running Manjaro (or considering it), or if you're interested in infrastructure automation, I highly recommend exploring this approach. The time invested in creating a robust automation playbook pays dividends every time you need to set up or recover a system.

## Resources

- **Project Repository**: [github.com/PauloPortugal/manjaro-playbook](https://github.com/PauloPortugal/manjaro-playbook)
- **Ansible Documentation**: [docs.ansible.com](https://docs.ansible.com)
- **Manjaro Linux**: [manjaro.org](https://manjaro.org)
- **Arch User Repository**: [aur.archlinux.org](https://aur.archlinux.org)

> Happy automating! May your systems always converge to their desired state.
{: .prompt-tip }
