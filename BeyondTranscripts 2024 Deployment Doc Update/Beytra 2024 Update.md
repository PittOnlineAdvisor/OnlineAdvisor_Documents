# Beyond Transcripts 2024 Update

Alex Zhou, Katelyn Donaty, David Simpkins. Fall 2024

## Current State of the Project

### StudentPaths / Online Advisor

Relevant Repositories:

- OnlineAdvisor_BackEnd_2024
  - The Spring backend of the App
- OnlineAdvisor_FrontEnd
  - Next.js Frontend
- OnlineAdvisor_ML
  - Main(?) Repository, ML models and predictions for the app
  - The SQL and Redis files will be created in the folder when running on Docker

For running the application, see [StudentPaths Docker Deployment](./StudentPaths%20Docker%20Deployment.md)

The minimum viable product is achieved and containerized. The app is ready for deployment. However, the beyondt account on Azure is currently lacking permission to attach private endpoint for the application, thus no sql server can be attached to the web app.

Pitt IT referred the team to contact security (Pitt Cybersecurity Team I persume), has not heard back from them since.  
Once the permission issue is sorted out, the app should be ready to be deployed as Docker containers. There would likely be some endpoint changes in the app (front end request url and backend API endpoint). But as there is no known domain for the app, we cannot make the changes.

### Concept Progression Maps (beytra repos)

Relevant Repositories:

- beytra-2024
  - Dev repository for the application, both front end and backend are in this repo
  - beytra-fe and beytra-be respectively, also built with Spring
  - Unit tests for the backend is in the `test` branch
- beytra-prod
  - Contains code currently deployed to Azure
  - beytra-lms contains the frontend, beytra-be contains the backend
- beytra-sso
  - Contains some code of attempting settup SSO on the app, there are also similar code in the dev repo's backend
- beytra-canvas
  - Code for Canvas API calls, also contains 2 scripts that will download the grades as CSV files

For running the application, see [CPM Docker Deployment](./CPM%20Docker%20Deployment.md).

The webapp is currently deployed to Azure, although with some bugs (see next section for details). A local dev environment can be done by running the backend alongside the Postgres server on Docker, and running the front end locally.

- Frontend compiles extremely slow if ran from Docker

The frontend has the code to upload csv files to be parsed into concept maps. The csv files can be used to indicate concepts, links, assignments, etc. For testing, the `public/assets/csvs/sample＿xxx.csv` (replace xxx with the content of uploda) in the public folder of the front end repository can be used. *Do not used the 1550 folder's files, it will trigger parsing errors in the web app*.

After uploading assignments and student's grades, the node should be colour coded, with green nodes being concepts that are learned well and red nodes being the opposite.

__See [Beytra Report](./Beytra%20Report.docx) for Azure Instructions__

## Known Bugs & Missing Features

- Student Paths
  - Good for deployment
- Concept Progression Maps
  - SSO Code not implemented
  - Azure server has some issues
    - The backend is not able to find the SQL server, possibly an endpoint connection issue but did not find a fix as if the end of the current semester
    - Develop new features locally if possible
  - The Canvas Fetch requests are blocked due to CORS configuration
    - Canvas repsonse do not contain a CORS header as the data is not meant for the public
    - Possibly need to discuss with Pitt IT about the issue, need to be whitelisted or there needs to be some domain changes for the app
    - The quiz fetch code is extremely unoptimised, could use some optimisation attempts if there exists some API calls that allows to retrieve specific points for each quiz question submission
