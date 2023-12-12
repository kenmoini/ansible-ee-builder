# Ansible Execution Environment Builder

This repository hosts a series of different [Execution Environments](https://docs.ansible.com/automation-controller/latest/html/userguide/execution_environments.html) to be used with Ansible Automation Platform 2.

Execution Environments contain all the needed Ansible Collections, Roles, supplementary binaries, Python modules, and other assets in a containerized format.

Building them is rather easy with `ansible-builder` and this repository further eases the process by leveraging GitHub Actions and Azure DevOps Pipelines to build them and push them to a container registry such as [Quay.io](https://quay.io).

There is a base/template GitHub Action in this repository under `.github/workflows/build-ee-base.yml` that you can use in your own GitHub Repos simply by referencing it.

## Available Execution Environments

- **Default EE** - `quay.io/kenmoini/ansible-default-ee` - An Execution Environment with some basic collections, a good place to get started. 
- **Kubernetes EE** - `quay.io/kenmoini/ansible-k8s-ee` - For use when automating against Kubernetes/OpenShift clusters
- **Nutanix EE** - `quay.io/kenmoini/ansible-nutanix-ee` - A good base for automating Nutanix via Prism Central
- **Windows EE** - `quay.io/kenmoini/ansible-windows-ee` - A basic Execution Environment for automating Windows targets
- **ZTP EE** - `quay.io/kenmoini/ansible-ztp-ee` - An EE that can be used for OpenShift Zero Touch Provisioning practices

## Documentation

- [Azure DevOps Setup and Usage](docs/azure-devops-setup.md)

## TODO

- Add base extension docs for GitHub Actions
