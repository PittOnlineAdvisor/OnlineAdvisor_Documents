# Concept Progreession Map User Guide

## Requirements and Local Deployment

- Git
- Java 1.17
- Docker
- Node.js 18+
- npm

- Backend Docker Deployment

```bash
git clone https://github.com/PittOnlineAdvisor/beytra-2024.git
docker compose up 
```

- Front End

The frontend has performance issues if hosted on Docker, for local testing simply host it locally.

```bash
git clone https://github.com/PittOnlineAdvisor/beytra-lms.git
cd beytra-lms
npm install i
npm run dev
```

- Enter localhost:3000 in browser to view site

## Usage

### Note

Sample data for the webapp is included in the github repositories, see `beytra-2024/beytra-fe/public/assets/csvs/` to find the sample files

### Loading Course Concepts

- After the webpage load, enter the new course name whose concepts you want to check for in the `Select A Course To Display` textbox
  - Currently the Postgres server does not have persistent storage, if the container is shut down all data is lost
  - If the container has not be shutdown between sessions, previously imported courses will be available to be selected
- Enter course name and description (csv files shown for corresponding sample files in the repository)
- Upload course topics
  - `sample_course_topics.csv`
- Upload course concepts
  - `sample_course_concepts.csv`
- Upload course concept links
  - `sample_course_concept_links.csv`
- If the data is all valid, the application will store information to the backend, and load the concept map

### Concept Map Page

- After the requried concepts files for a course is uploaded, the concept map page will be loaded with the map in 3D
  - The display can be switched to 2D with the `Switch to 2D` button.
- At the start, the concpet maps will have no content other than the concepts loaded into it.
- To populate it, corresponding assignments and student grades has to be uploaded.
- The assignment upload process is fairly similar to creating the concept map itself
  - First, upload the assignment information
    - `sample_assignments.csv`
  - The Questions information
    - `sample_questions.csv`
  - The link between questions and concepts
    - `sample_question_concepts.csv`
- After the assignment information is uploaded, student information can be uploaded to populate the concept map
  - `sample_student_question_grades.csv`
- To pull grades from canvas, see the README file in the `beytra-canvas` repository
  - The grades will be downloaded as a csv file that can be used to upload to the web app manually
- After uploading student grades, the concept node should be coloured from red to green depending on the score on the assignments relating to each concepts
