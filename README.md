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

There are several assumptions made about the following model:
* Each member of the development team charges the same price.
* Each member of the team has unlimited time meaning there will be no bottlenecks if a 2 tasks can be completed simultaneously but require the same team member.
* Each task can be immediately started assuming the preceding tasks are completed.

## Part 3: Programming
In order to set up this linear program within Python, 2 dictionaries were initialized: *tasks* and *predecessors*. 

_Tasks_ represents the description of each task to be compelted as well as the three time estimates for how long the task will take.

_predecessors_ has the same tasks and holds the TaskID of each task that must be completed before the current task can be started.

````python
from pulp import *
import matplotlib.pyplot as plt
from pulp import LpMinimize, LpProblem, LpVariable, lpSum, value

tasks = {
    "A": {"name": "Describe product", "best_case": 10, "expected": 18, "worst_case": 25},
    "B": {"name": "Develop marketing strategy", "best_case": 6, "expected": 20, "worst_case": 22},
    "C": {"name": "Design brochure", "best_case": 3, "expected": 6, "worst_case": 10},
    "D1": {"name": "Requirements analysis", "best_case": 15, "expected": 30, "worst_case": 60},
    "D2": {"name": "Software design", "best_case": 10, "expected": 15, "worst_case": 20},
    "D3": {"name": "System design", "best_case": 9, "expected": 10, "worst_case": 18},
    "D4": {"name": "Coding", "best_case": 65, "expected": 100, "worst_case": 170},
    "D5": {"name": "Write documentation", "best_case": 25, "expected": 30, "worst_case": 50},
    "D6": {"name": "Unit testing", "best_case": 25, "expected": 50, "worst_case": 72},
    "D7": {"name": "System testing", "best_case": 20, "expected": 23, "worst_case": 30},
    "D8": {"name": "Package deliverables", "best_case": 6, "expected": 10, "worst_case": 20},
    "E": {"name": "Survey potential market", "best_case": 20, "expected": 50, "worst_case": 70},
    "F": {"name": "Develop pricing plan", "best_case": 10, "expected": 15, "worst_case": 20},
    "G": {"name": "Develop implementation plan", "best_case": 15, "expected": 26, "worst_case": 35},
    "H": {"name": "Write client proposal", "best_case": 9, "expected": 15, "worst_case": 20}
}


predecessors = {
    "A": [],
    "B": [],
    "C": ["A"],
    "D1": ["A"],
    "D2": ["D1"],
    "D3": ["D1"],
    "D4": ["D2", "D3"],
    "D5": ["D4"],
    "D6": ["D4"],
    "D7": ["D6"],
    "D8": ["D5", "D7"],
    "E": ["B", "C"],
    "F": ["D8", "E"],
    "G": ["A", "D8"],
    "H": ["F", "G"]
}
````
The next piece of the Python script is the "meat" of the code. A critical path minimization problem is initialized here and the activities as well as predecessors are fed into the funciton as inputs. For each activity, an equation is set up with the given time constraints and predecessors and the optimization problem is set up to minimize the total number of time that is needed to complete the entire project. All data is output onto the display and a gantt chart is generated that displays the total time needed to complete the problem under the three scenarios.

````python
def critical_path_analysis(activities, predecessors, scenario="expected"):
    # Create a list of the activities
    activities_list = list(activities.keys())
    # Create the LP problem
    prob = LpProblem("Critical Path", LpMinimize)

    # Create the LP variables
    start_times = {activity: LpVariable(f"start_{activity}", 0, None) for activity in activities_list}
    end_times = {activity: LpVariable(f"end_{activity}", 0, None) for activity in activities_list}

    # Add the constraints
    for activity in activities_list:
        # Access the duration for the current scenario
        duration = activities[activity][scenario]  
        prob += end_times[activity] == start_times[activity] + duration, f"{activity}_duration"
        for predecessor in predecessors[activity]: 
            prob += start_times[activity] >= end_times[predecessor], f"{activity}_predecessor_{predecessor}"

    # Set the objective function
    prob += lpSum([end_times[activity] for activity in activities_list]), "minimize_end_times"

    # Solve the LP problem
    status = prob.solve()

    # Print the results
    print("Critical Path time:")
    for activity in activities_list:
        if value(start_times[activity]) == 0:
            print(f"{activity} starts at time 0")
        if value(end_times[activity]) == max([value(end_times[activity]) for activity in activities_list]):
            print(f"{activity} ends at {value(end_times[activity])} hours in duration")

    # Print solution
    print("\nSolution variable values:")
    for var in prob.variables():
        if var.name != "_dummy":
            print(var.name, "=", var.varValue)
            
    # Prepare data for Gantt chart
    gantt_data = []
    for activity in activities_list:
        start = value(start_times[activity])
        end = value(end_times[activity])
        gantt_data.append((activity, start, end - start))

    # Plot Gantt chart
    fig, ax = plt.subplots(figsize=(15, 8))
    for i, (task, start, duration) in enumerate(gantt_data):
      task_label = f"{task}: {activities[task]['name']}"
      ax.barh(task_label, duration, left=start, align='center')

    ax.set_xlabel('Time (Hour)')
    ax.set_title(f'{scenario.upper()} Gantt Chart')
    plt.show()
````

The following code was used to call the above function and display the optimization results for all three scenarios.
````python
# Calculate and print the critical path for each scenario
scenarios = ["best_case", "expected", "worst_case"]
for scenario in scenarios:
    print(f"--- {scenario.upper()} SCENARIO ---")
    critical_path_analysis(tasks, predecessors, scenario)
    print()
````
## Part 4: Solution
Based off the results of the program here are the following results:

**Best Case Scenario**
Total Time/cost: 175 hours / $105,000 total
![download](https://github.com/user-attachments/assets/9b546481-b4de-4d83-b6d4-cc31e2d8cbc4)


**Expected Scenario**
Total Time/Cost: 287 hours / $172,000
![download](https://github.com/user-attachments/assets/2e8a3346-0703-4777-b2c9-a9e666937840)


**Worst Case Scenarip**
Total Time/Cost: 452 hours / $271,000
![image](https://github.com/user-attachments/assets/63a8a8a7-be60-4d1c-99f7-a4161d70ca9e)

Based off these solutions, the project could take anywhere from 19 to 51 days to complete (assuming 9-hour work days)

## Part 5: Overview
