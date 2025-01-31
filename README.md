# MSDS460Assignment2

## Assignment Description
After many years of working for others, you have decided to start your own data science and software engineering firm. You will offer your services to other organizations as a sole proprietor and independent contractor. A first step in obtaining clients is often to respond to requests for proposal. 

This assignment concerns a development project. Suppose a group of restaurant owners in Marlborough, Massachusetts is requesting a project proposal. They want to know how fast you can develop a new software product and how much it will cost.

You need to propose a consumer-focused recommendation system for more than one hundred restaurants in Marlborough, Massachusetts, as shown in the enclosed file: restaurants-v001.json Download restaurants-v001.json. Data for the project will consist of Yelp reviews of these restaurants. This list of restaurants should be updated monthly, with Yelp reviews updated daily. Given the limitations of open-source Yelp reviews, it is likely that a contract will be obtained to access large quantities of Yelp data through its GraphQL API.

The client requires selected software components for the project. In particular, the client asks that the product recommendation system be implemented using Alpine.js and Tailwind on the frontend, a GraphQL API, and a Go web and database server on the backend. Go, Python, or R may be employed for recommender system analytics on the backend, with persistent storage provided by PostgreSQL, EdgeDB, or PocketBase. The system should be accessible from any modern web browser and may be hosted on a major cloud platform such as Amazon Web Services (AWS), Microsoft Azure, or Google Cloud Platform (GCP). 

While you expect to be involved in all aspects of the recommender system, you realize that the nature of the project calls for a software development team. You will need to hire people to fill various roles, including frontend developer, backend developer, data engineer, and database administrator, while you fill data science and project management roles.  

The project consists of the eight general tasks, and one of these general tasks, develop product prototype, comprises eight software development subtasks. An Excel spreadsheet shows fifteen tasks with their immediate predecessors: project-plan-v003.xlsx Download project-plan-v003.xlsx  Numerous columns are defined for workers assigned to the project. These columns may prove useful in activity/task scheduling in the future, but they are not used in the critical path analysis itself. Entries in these columns can be integers indicating the numbers of workers of each type assigned to the activity.

Determine best-case, expected, and worst-case estimates for the number of hours needed for each of the sixteen tasks. Also, determine an hourly rate associated with each worker role. These rates can be hourly rates (excluding benefits) that you would expect people in the roles to earn. Assume that all members of the team, like yourself, are working as independent contractors.
 
 ---
**Part 1: Problem setup.**  Describe the work that you have completed, posting the updated Excel spreadsheet of tasks, completion times, per-hour costs, and persons responsible. Describe any areas of uncertainty regarding project components, time, or cost estimates. Draw a directed graph diagram for the project. Note tasks that can be completed in parallel.

**Part 2: Model specification.** Specify a linear programming model for the project plan. Show how time and cost estimates play into the specification. In specifying the objective function, you can make the simplifying assumption that all contributors to the project charge the same hourly rate. That is, a minimum total time solution will be the same as a minimum total cost solution.

**Part 3: Programming.** Implement the linear programming problem using Python PuLP or AMPL. Provide the program code and output/listing as plain text files, posting within a GitHub repository dedicated to this assignment. 

**Part 4: Solution.** Solve the project plan using best-case, expected, and worst-case time estimates with a minimum-time or minimum-cost objective. Describe your solution. Identify the critical path for the project under each scenario. Construct Gantt charts for the best-case, expected, and worst-case solutions. Note that Microsoft provides Gantt chart templates for ExcelLinks to an external site..

**Part 5: Overview.** Write an overview of the project for the prospective client. Ignoring costs associated with software licensing and cloud hosing, what would you charge for the project? When would you expect to be delivering the product prototype? How soon could you deliver the product prototype if additional independent contractors were added to the mix? As part of your discussion, consider other methods for dealing with uncertainty associated with the completion of activities. For example, would you consider using stochastic programming and/or Monte Carlo simulation?

## Part 1: Problem Setup
The following assignment is a Critical Path problem where the objective of the problem is to find the minimal amount of time to complete all tasks deemed necessary for the completion and successful delivery of a restaurant reeccomendation applcation. After each essential task was identified, time estimates in terms of hours and preceding steps. The entire tablle used to specifiy the project details can be in [WONG_project-plan-v003.xlsx](WONG_project-plan-v003.xlsx) found or below:

| taskID | task                        | predecessorTaskIDs | bestCaseHours | expectedHours | worstCaseHours | projectManager | frontendDeveloper | backendDeveloper | dataScientist | dataEngineer |
|--------|-----------------------------|--------------------|---------------|---------------|----------------|----------------|-------------------|------------------|---------------|--------------|
| A      | Describe product            |                    | 10            | 18            | 25             | x              |                   |                  |               |              |
| B      | Develop marketing strategy  |                    | 6             | 20            | 22             |                |                   | x                |               |              |
| C      | Design brochure             | A                  | 3             | 6             | 10             | x              |                   |                  |               |              |
| D      | Develop product prototype   |                    |               |               |                |                |                   |                  |               |              |
| D1     | Requirements analysis       | A                  | 15            | 30            | 60             |                |                   | x                | x             |              |
| D2     | Software design             | D1                 | 10            | 15            | 20             | x              |                   |                  |               |              |
| D3     | System design               | D1                 | 9             | 10            | 18             |                |                   |                  |               |              |
| D4     | Coding                      | D2, D3             | 65            | 100           | 170            |                |                   |                  | x             | x            |
| D5     | Write documentation         | D4                 | 25            | 30            | 50             |                |                   |                  |               |              |
| D6     | Unit testing                | D4                 | 25            | 50            | 72             |                |                   |                  | x             |              |
| D7     | System testing              | D4                 | 20            | 23            | 30             |                |                   |                  |               | x            |
| D8     | Package deliverables        | D5, D7             | 6             | 10            | 20             |                |                   |                  |               |              |
| E      | Survey potential market     | B, C               | 20            | 50            | 70             | x              |                   |                  | x             |              |
| F      | Develop pricing plan        | D8, E              | 10            | 12            | 15             |                |                   |                  |               | x            |
| G      | Develop implementation plan | A, D8              | 15            | 26            | 35             |                |                   |                  |               | x            |
| H      | Write client proposal       | F, G               | 9             | 15            | 20             | x              |                   |                  |               |              |

A visual representation of workflow of each task can be seen below in this [Visio diagram](Wong_Assignment4.vsdx):
![image](https://github.com/user-attachments/assets/c7f55a7d-7863-497d-bf16-0006b4f54765)

The cost of labor per hour was estimated to be $800/ hr (representing the time from project start to finish. This does not represent the sum of all hours worked by each project member).

## Part 2: Model Specification
Given the information above, a model was developed in Python utilizing the PuLP package to model the project timeline as a linear program. Furthermore, matplotlib was used to develop Gantt charts based off the 3 scenarios. The entire code can be found in this repository ([.pf file](wong_msds460_Assignment2.py)/[.ipynb file](Wong_MSDS460_Assignment2.ipynb)).

## Part 3: Programming

## Part 4: Solution

## Part 5: Overview
