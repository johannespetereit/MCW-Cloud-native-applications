![Microsoft Cloud Workshop](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png 'Microsoft Cloud Workshop')

<div class="MCWHeader1">
Cloud-native applications
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
November 2021
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2021 Microsoft Corporation. All rights reserved.

**Contents**

<!-- TOC -->

- [Cloud-native applications before the hands-on lab setup guide](#cloud-native-applications-before-the-hands-on-lab-setup-guide)
  - [Overview](#overview)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
    - [Task 1: Set up Azure Cloud Shell](#task-1-set-up-azure-cloud-shell)
    - [Task 2: Download Starter Files](#task-2-download-starter-files)
    - [Task 3: Create a GitHub repository](#task-3-create-a-github-repository)
    - [Task 6: Build Docker Images](#task-6-build-docker-images)

<!-- /TOC -->

# Cloud-native applications before the hands-on lab setup guide

## Overview

Before the hands-on lab, you will need to prepare the environment by deploying the database and the application locally on a virtual machine using Docker and MongoDB. You will also need to fork the GitHub repository containing the lab to your own GitHub account to be able to set up the CI/CD pipeline.

## Requirements
2. An account in Microsoft [GitHub](https://github.com).

## Before the hands-on lab

**Duration**: 60 minutes

You should follow all the steps provided in this section _before_ taking part in the hands-on lab ahead of time as some of these steps take time.

### Task 1: Set up Azure Cloud Shell

1. Open a cloud shell by selecting the cloud shell icon in the menu bar.

   ![The cloud shell icon is highlighted on the menu bar.](media/b4-image35.png "Cloud Shell")

2. The cloud shell opens in the browser window. Choose **Bash** if prompted or use the left-hand dropdown on the shell menu bar to choose **Bash** from the dropdown (as shown). If prompted, select **Confirm**.

   ![This is a screenshot of the cloud shell opened in a browser window. Bash was selected.](media/b4-image36.png "Cloud Shell Bash Window")

### Task 2: Download Starter Files

> **Note**: You will need access to your Azure subscription via Azure Cloud Shell to proceed further in the workshop.  If you don't have a cloud shell available, refer back to [Task 1: Set up Azure Cloud Shell](#task-1-set-up-azure-cloud-shell) for set up instructions.

1. Check out the starter files from the MCW Cloud-native applications GitHub repository and detach them from the existing remote repository via the following commands:

   ```bash
   cd ~
   git clone \
      --depth 1 \
      --branch main \
      https://github.com/microsoft/MCW-Cloud-native-applications.git \
      MCW-Cloud-native-applications
   cd MCW-Cloud-native-applications
   ```

   > **Note**: If you do not have enough free space, you may need to remove extra files from your Cloud Shell environment.  Try running `azcopy jobs clean` to remove any `azcopy` jobs and data you do not need.

   ![In this screenshot of a Bash window, git clone has been typed and run at the command prompt. The output from git clone is shown.](media/b4-2019-09-30_21-25-06.png "Bash Git Clone")

### Task 3: Create a GitHub repository

FabMedical has provided starter files for you. They have taken a copy of the websites for their customer Contoso Neuro and refactored it from a single node.js site into a website with a content API that serves up the speakers and sessions. This refactored code is a starting point to validate the containerization of their websites. Use this to help them complete a POC that validates the development workflow for running the website and API as Docker containers and managing them within the Azure Kubernetes Service environment.

1. Open a web browser and navigate to <https://www.github.com>. Log in using your GitHub account credentials.

2. In the upper-right corner, expand the user drop-down menu and select **Your repositories**.

    ![The user menu is expanded with the Your repositories item selected.](media/2020-08-23-18-03-40.png "User menu, your repositories")

3. Next to the search criteria, locate and select the **New** button.

    ![The GitHub Find a repository search criteria is shown with the New button selected.](media/2020-08-23-18-08-02.png "New repository button")

4. On the **Create a new repository** screen, name the repository **Fabmedical** and select the **Create repository** button.

    ![Create a new repository page with Repository name field and Create repository button highlighted.](media/2020-08-23-18-11-38.png "Create a new repository")

5. On the **Quick setup** screen, copy the **HTTPS** GitHub URL for your new repository, paste this into notepad for future use.

    ![Quick setup screen is displayed with the copy button next to the GitHub URL textbox selected.](media/2020-08-23-18-15-45.png "Quick setup screen")


### Task 6: Build Docker Images

1. Navigate to the `content-api` directory and build the `content-api` container image using the Dockerfile in the directory. Note how the deployed Azure Container Registry is referenced. Replace the `SUFFIX` placeholder in the command.

   ```bash
   cd ~/Fabmedical/content-api
   docker image build -t fabmedical[SUFFIX].azurecr.io/content-api:latest .
   ```

2. Repeat this step for the `content-web` image, which serves as the application front-end.

   ```bash
   cd ~/Fabmedical/content-web
   docker image build -t fabmedical[SUFFIX].azurecr.io/content-web:latest .
   ```

3. Observe the built Docker images by running `docker image ls`. The images were tagged with `latest`, but it is possible to use other tag values for versioning.

   ![This image demonstrates the tagged Docker images: content-api and content-web.](./media/docker-images.png "Tagged Docker images")

4. Log in to Azure Container Registry using `docker login fabmedical[SUFFIX].azurecr.io`. Fetch the credentials from the **Access keys** tab of the ACR instance in the Azure portal.

   ![This image demonstrates the credentials for Azure Container Registry.](./media/acr-credentials.png "ACR credentials")

5. Push the two images you built.

   ```bash
   docker image push fabmedical[SUFFIX].azurecr.io/content-api:latest
   docker image push fabmedical[SUFFIX].azurecr.io/content-web:latest
   ```

You should follow all steps provided _before_ performing the Hands-on lab.
