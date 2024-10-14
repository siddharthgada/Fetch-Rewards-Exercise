What are the requirements?:
- Review unstructured JSON data and diagram a new structured relational data model
- Generate a query that answers a predetermined business question
- Generate a query to capture data quality issues against the new structured relational data model
- Write a short email or Slack message to the business stakeholder
Please let us know which SQL dialect you are using and include any code, notes, etc.. that helped you develop your answers. Showing your work can only help you!

First: Review Existing Unstructured Data and Diagram a New Structured Relational Data Model:
Review the 3 sample data files provided below. Develop a simplified, structured, relational diagram to represent how you would model the data in a data warehouse. The diagram should show each table’s fields and the joinable keys. You can use pencil and paper, readme, or any digital drawing or diagramming tool with which you are familiar. If you can upload the text, image, or diagram into a git repository and we can read it, we will review it!

Second: Write queries that directly answer predetermined questions from a business stakeholder:
Write SQL queries against your new structured relational data model that answer at least two of the following bullet points below of your choosing. Commit them to the git repository along with the rest of the exercise.

Note: When creating your data model be mindful of the other requests being made by the business stakeholder. If you can capture more than two bullet points in your model while keeping it clean, efficient, and performant, that benefits you as well as your team.

- What are the top 5 brands by receipts scanned for most recent month?
- How does the ranking of the top 5 brands by receipts scanned for the recent month compare to the ranking for the previous month?
- When considering average spend from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
- When considering total number of items purchased from receipts with 'rewardsReceiptStatus’ of ‘Accepted’ or ‘Rejected’, which is greater?
- Which brand has the most spend among users who were created within the past 6 months?
- Which brand has the most transactions among users who were created within the past 6 months?

Third: Evaluate Data Quality Issues in the Data Provided
Using the programming language of your choice (SQL, Python, R, Bash, etc...) identify as many data quality issues as you can. We are not expecting a full blown review of all the data provided, but instead want to know how you explore and evaluate data of questionable provenance.

Commit your code and findings to the git repository along with the rest of the exercise.

Fourth: Communicate with Stakeholders:
Construct an email or slack message that is understandable to a product or business leader who isn’t familiar with your day to day work. This part of the exercise should show off how you communicate and reason about data with others. Commit your answers to the git repository along with the rest of your exercise.

- What questions do you have about the data?
- How did you discover the data quality issues?
- What do you need to know to resolve the data quality issues?
- What other information would you need to help you optimize the data assets you're trying to create?
- What performance and scaling concerns do you anticipate in production and how do you plan to address them?
