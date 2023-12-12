# Azure DevOps Setup

This repository can be used in Azure DevOps with Azure DevOps Pipelines to build the Execution Environments and push them to a Container Registry.

## Prerequisites

- Access to a Container Registry with push/write permissions.  I suggest using [Quay.io](https://quay.io).  Create a few Repositories, add a Robot Account to them with Write permission, and you can use it easily across all these Pipelines.
- Permission to create/import Azure DevOps Projects/Repos.
- Permission to run Azure DevOps Pipelines.

## Setup

1. Log into the Azure Portal: https://portal.azure.com
2. Access Azure DevOps: https://dev.azure.com
3. [Optional] Create a new ADO Organization
4. [Optional] Create a new **Project** in an Organization you have access/permission to

![Optionally, create an ADO Project](images/ado-project-home.jpg)
![Give the new ADO Project a Name](images/ado-create-project.jpg)

5. Navigate to the **Repos** section of the ADO Project, click the **Import** button to Import a Repository

![Click the Import button](images/ado-project-repo-home-empty.jpg)

6. Import a Git type repository, you can use this upstream repo as the **Clone URL Source**: `https://github.com/kenmoini/ansible-ee-builder`

![Import a Git repo from the upstream source](images/ado-import-git-repo.jpg)

7. Once imported, you should see the **Files** section of the ADO Repo populated.

![Confirm that the Files section of the Repo has been populated](images/ado-repo-imported.jpg)

8. Navigate to the **Pipelines** section, click the kebab menu (3 vertical dots) next to the Create Pipeline button, select the **Import a pipeline** option

![Navigate to Pipelines, select Import a pipeline](images/ado-repo-imported.jpg)

9. When importing a pipeline, in the **Connect** section choose **Azure Repos Git** for the source

![Select Azure Repos Git for the Connection source](images/ado-pipeline-connect.jpg)

10. **Select** the repository that was just imported into ADO Repos

![Select the previously imported repo](images/ado-pipeline-select-repo.jpg)

11. **Configure your pipeline** with an **Existing Azure Pipelines YAML file**

![Configure with an Existing Azure Pipelines YAML file](images/ado-pipeline-existing-config.jpg)

12. In the pane that opens to the right, select the path to the `/execution-environments/default-ee/azure-pipelines.yml` file

![Select the Path of the first Pipeline](images/ado-pipeline-default-ee-path.jpg)

13. **Review the Pipeline YAML**, replace line 15 where it defines the `REGISTRY_PATH` parameter - use your Quay username or whatever path to the target Repository in your Container Registry that you set up.  Click **Save**.

![Modify the Pipeline as needed and click Save](images/ado-pipeline-save.jpg)

14. Once the Pipeline is imported, make sure to Rename it

![Rename the imported Pipeline](images/ado-pipeline-rename.jpg)

15. Repeat steps 8-14 for the remaining Azure Pipelines for the other Execution Environments if you'd like, eg:
    1.  `/execution-environments/k8s-ee/azure-pipelines.yml`
    2.  `/execution-environments/nutanix-ee/azure-pipelines.yml`
    3.  `/execution-environments/windows-ee/azure-pipelines.yml`
    4.  `/execution-environments/ztp-ee/azure-pipelines.yml`
16. In the ADO Pipelines section, navigate to **Library** and create a new **Variable Group**

![Navigate to Pipelines > Library and create a new Variable Gropu](images/ado-pipeline-library.jpg)

17. **Name** the Variable Group `RegistrySecrets` - if this name is modified the base pipeline template also needs to be modified to match.  Add the `REGISTRY_USERNAME` and `REGISTRY_PASSWORD` variables and make sure to click the Lock button to the right of their rows to **make them Secrets**.

![Give the Variable Group a name, and add your registry credentials as a secret](images/ado-pipeline-variable-group.jpg)

18. Click **Save** to set this Variable Group

![Click Save to store the Variable Group](images/ado-pipeline-variable-group-save.jpg)

19. In the same RegistrySecrets Variable Group, click the **Pipeline permissions** button

![Click the Pipeline permissions button](images/ado-pipeline-vargroup-permissions.jpg)

20. **Add the Pipelines** to allow them to use the Variable Group

![Add the Pipeline to give it access](images/ado-pipeline-add-permission.jpg)

21. **Repeat** this for the remaining Pipelines

![Repeat for all the Pipelines](images/ado-pipeline-all-repo-permissions.jpg)

22. In the ADO Pipelines page, you can now expand the kebab menu and select **Run pipeline**

![Run the pipeline](images/ado-pipelines-run.jpg)

23. These ADO Pipelines will also be triggered whenever their Execution Environment files have changes pushed to them - as well as scheduled to run weekly at midnight on Sunday.