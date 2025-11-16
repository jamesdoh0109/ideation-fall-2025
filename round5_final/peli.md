# Final Project Proposal: Peli


**Team Name**: Peli
**Submission Date**: November 15th, 2025
**GitHub Organization**: https://github.com/nets2130-fall2025-group9/peli
(let us know if you want access, it’s currently a private repo)


---


## Team Information


### Team Members


| Name | PennKey | Primary Role(s) | Secondary Skills |
|------|---------|----------------|------------------|
| James Doh | dohj0109 | Frontend, UI/UX | Backend Dev |
| Eric Lee | ericclee | API Integration | Frontend UI/UX |
| Joshua Lee | jlee0902 | Data Analysis | Applied ML, Data Visualization |
| Andrew Park | andrewpp | Aggregation | Backend |
| Katherine Yue | kyue | Backend | DB Design, Analysis |


### Team Skills Inventory


**Skills we have:**
- Frontend Dev: James Doh/dohj0109 & Eric Lee/ericclee
- Backend Dev: James Doh/dohj0109 & Katherine Yue/kyue
- Data Analysis: Katherine Yue/kyue & Joshua Lee/jlee0902
- Deployment: Andrew Park/andrewpp


**Skills we need to learn/acquire:**
- Scheduling / Scraping: To get the menu information every morning - James Doh, Katherine Yue
- Scaling: For potentially thousands of users, there may be more resources needed to handle the load - Andrew Park
- (Stretch) Mobile Development: Some students might find it easier to put data on their phone and not their laptops - Eric Lee


**External resources we might need mrow:**
- Penn Single Sign-On: to only allow Penn students to access the platform - Status: NOT REQUESTED
- Penn Dining Information: API access to daily menu and other dining hall information - Status: NOT REQUESTED
- Google Maps: API for map interface (may be able to use a free resource instead) - Status: NOT REQUESTED


### Team Availability for TA Meetings


**Any week should work:**


_List all time slots when the ENTIRE team can meet with a TA. Use Eastern Time. Format: Day, Time-Time_


- Monday: only with 3 people, 6:00PM - 9:00PM
- Tuesday: all 5 people, 5:00 - 5:45PM
- Wednesday: not available, 2 or less people available at a time
- Thursday: not available, 2 or less people available at a time
- Friday: not available, 2 or less people available at a time


**Preferred meeting duration**: 30 min


**Meeting format preference**: Zoom


**Primary contact for scheduling**: Katherine Yue (kyue@seas.upenn.edu)


---


## Project Overview


### Project Connection to Round 4


**Round 4 Decision**: PIVOTING


**Original idea from**: Instructor (mainly came from round 4).


**How the idea evolved**:
_Briefly describe how your project changed from its original conception through Rounds 1-4 to this final proposal_


While we all had different ideas throughout these rounds, there were many pain points such as scope, incentives, and more that would be difficult to work through without giving up some other key component. Thus, this project is something that we believe has more of these issues ironed out – for example, the scope is feasible, there are more incentives with our targeted audience, and more. All in all, this project idea was the one we were most excited about, and thus we decided to work on this project.


### Problem Statement


_Refined from your Round 4 decision_


For many Penn students, eating good food is an important part of our daily lives. There have been so many times where meals were met with disappointment despite checking the menu beforehand, and students share this sentiment with each other daily.


### One-Sentence Pitch


A platform that allows students to rate dining hall menus and see what others think before swiping in.


### Target Users


**End Users**: Penn Students


**Crowd Workers**: Penn Students


**Scale**: At least 50 menu item ratings from 10+ students for demo purposes.


### Project Type


- [ ] Human computation algorithm
- [ ] Social science experiment with the crowd
- [ ] Tool for crowdsourcing (requesters or workers)
- [X] Business idea using crowdsourcing
- [ ] Other: [specify]


---


## System Architecture


### Flow Diagram


User rating submission → QC module (duplicate + profanity checks) → QC-approved ratings table → Aggregation module (compute averages + confidence) → Aggregated results table → Frontend displa


**Flow diagram location**: [e.g., `docs/flow-diagram.pdf` or embed image here]


Your flow diagram MUST clearly show:
- [ ] Where/when the crowd touches the data
- [ ] Your quality control module
- [ ] Your aggregation module
- [ ] Data flow between components
- [ ] What happens before crowd involvement
- [ ] What happens after crowd contribution


**If you haven't created it yet**: Describe in words the major components and their relationships:


Separate from Users:
1. Asynchronously, the dining hall menus are pulled each morning.
2. Previous ratings are saved in the database according to the title of the item.
3. Database is updated with daily menu.
The user logs into the system (authentication component).


User Flow:
1. User logs in (authentication module).
2. User views the menu items on either the map interface or list (user touches the data).
3. User rates food items they’ve eaten (user can update the data).
4. Not only do users only have one rating per meal window, but poor input is automatically filtered (QC).
5. Taking in this data, the ratings are updated through the DB and displayed (aggregation).


Before crowd involvement: Daily menu updates with the ratings culminated thus far (if the same menu item appears multiple times, it will indicate how frequently it’s available alongside the rating).


After crowd involvement: Ratings are updated and saved for the future.


### Major System Components


_List all major components with point values (1-4) indicating implementation complexity. Total should be 15-20 points._


| Component | Description | Points | Owner(s) | Dependencies |
|-----------|-------------|--------|----------|--------------|
| Authentication | penn-email based login | 2 | James Doh | none |
| Rating Interface | form interface to submit ratings | 3 | Eric Lee | authentication |
| Quality Control | filtering ratings | 4 | Katherine Yue | having ratings / data |
| Aggregation | aggregates ratings and confidence | 4 | Josh Lee | qc output |
| Scraper | fetches daily menu items from penn dining | 3 | Jun Hyun Park | none |


**Total Points**: [Sum - should be 15-20]
16


**Point allocation rationale**:
_If teaching staff questions your point distribution, explain your reasoning here_
Rating was based more on how crucial each component is to the quality of the core platform, which resulted in quality control and aggregation having the highest scores. Rating interface and scraper are more technically challenging, while authentication mainly relies on getting the SSO resource.


### Detailed Workflow


_Step-by-step description of how your system works from start to finish_


1. **Daily Menu Scrape**: A scheduled backend job automatically scrapes the Penn Dining website each morning and updates the database with that day’s menu items.
2. **Database Update:**: The backend compares scraped items with previously stored items, assigns unique item IDs per day, and prepares them for display to users.
3. **User Login**: A Penn student logs in through the authentication system to ensure only verified Penn users can submit ratings.
4. **Menu Browsing**: The user views dining hall items either through the map interface or a list view, selecting items they have eaten.
5. **Rating Submission**: The user submits a rating (1–5) and an optional comment for any item they consumed
6. **Quality Control Check**: The QC module validates the rating by detecting duplicates, checking for profanity or inappropriate text, and ensuring the input format is correct. Submissions that fail QC are rejected; valid ones move forward
7. **Aggregation**:  The aggregation module processes all QC-approved ratings for each menu item, computes the average rating, distribution, and confidence score, and updates the stored aggregated results


### Human vs. Automated Tasks


| Task | Performed By | Justification |
|------|--------------|---------------|
| Submitting food ratings | Human | Requires subjective judgment and descriptions only humans can experience and provide |
| Scraping daily menu itmes | Automated | Deterministic and repeatable task, objective |
| Browsing results | Human | Human decision-making is needed to choose where to eat, people’s preferences are different |
| Filtering inappropriate results | Automated | With things like duplication, hard to have humans comb through everything and can be automated |


---


## Quality Control Module


### QC Strategy Overview


Our quality control strategy focuses on preventing low-quality or malicious inputs before they ever enter the system, while also detecting and filtering any duplicate or invalid submissions. Since our platform collects subjective ratings from Penn students, we do not evaluate correctness, but we can ensure that submission behavior is genuine, non-malicious, and non-spammy.
To achieve this, the QC module applies two main layers of protection. First, we restrict what users can input by design—ratings can only be numbers 1–5, and text fields such as comments or dish descriptions are automatically scanned for profanity, slurs, or inappropriate content. Second, the module enforces duplicate detection, ensuring that each user may only submit one rating per dining hall item per meal window. Together, these mechanisms keep the dataset clean without adding friction that discourages participation.
This approach is appropriate for Peli because it balances simplicity with strong guardrails. Students are highly motivated to share honest opinions about dining halls, so intrusive QC methods (like gold-standard questions) are unnecessary. Instead, we focus on eliminating spam, repeated inputs, trolling, and offensive content so aggregated ratings remain trustworthy and safe to display on a public-facing interface.




### Specific QC Mechanisms


**Primary mechanism**: 
Each user gets only one rating per menu item per meal period
All text input is automatically cleaned and validated before acceptance
**Implementation details**:
- Input format: QC Module will receive 
{
  "user_id": "ericclee",
  "item_id": "Hill_FrenchToast_2025-11-13",
  "rating": 4,
  "comment": "pretty good",
  "timestamp": "2025-11-13T09:15:30Z"
}
- Processing:Duplicate detection: Check DB for an existing rating from the same user for the same item within the same meal window.If found → reject OR overwrite depending on UX decision (we plan to simply block duplicates). Input restriction & profanity filtering: Rating must be an integer 1–5. Comment, if provided, is scanned through a profanity dictionary (e.g., "banned words list" or regex). If inappropriate language is detected → block submission and ask user to revise. Sanity Checks: Minimum content length for comments (to prevent empty trolling inputs like “.” or spam). Timestamp verification (e.g., cannot rate breakfast item after dinner).


- Output format: 
{
  "qc_passed": true,
  "item_id": "...",
  "user_id": "...",
  "rating_cleaned": 4,
  "comment_cleaned": "pretty good"
}


- Threshold for acceptance: 
No duplicates
No profanity
Valid rating ∈ [1–5]
Valid comment length OR optional comment omitted
Rating submitted within valid meal window


**Additional mechanisms**:
- [ ] Gold standard questions
 - _How many? How distributed? Pass/fail criteria?_
- [ ] Majority voting across multiple workers
 - _How many workers per task? Tie-breaking?_
- [X] Attention checks
 - Very lightweight: Prevent submitting instantly after page load. Ensure comment (if present) meets minimal length
- [ ] Reputation system
 - _How do workers build reputation?_
- [X] Statistical outlier detection
 - Optional future feature; not part of initial QC design.
- [ ] Other: [specify and explain]


### QC Module Code Plan


**Location in repo**: [e.g., `src/qc/quality_control.py`]


**Key functions/classes**:
1. is_duplicate(user_id, item_id, meal_window)
 Checks database for prior rating submissions by same user.
2. check_profanity(comment)
 Runs comment text against banned-word list + regex patterns.
3. validate_rating_format(payload)
 Ensures rating is 1–5, proper types, valid timestamp.
4. qc_process(payload)
 Full QC pipeline combining duplicate detection, profanity filtering, and format validation.




**Input data format**:
```
{
  "user_id": "kyue",
  "item_id": "KingsCourt_Waffles_2025-11-13",
  "rating": 5,
  "comment": "amazing!"
}
```
**Output data format**:
```
{
  "qc_passed": true,
  "user_id": "kyue",
  "item_id": "KingsCourt_Waffles_2025-11-13",
  "rating_cleaned": 5,
  "comment_cleaned": "amazing!"
}
```
**Sample scenario**:
_Walk through a concrete example of your QC module in action_


[Example: "Worker A labels image as 'cat', Worker B labels as 'dog', Worker C labels as 'cat'. QC module applies majority voting, outputs 'cat' with confidence 0.67, flags disagreement for potential review."]


User A tries to rate the same item twice. QC checks DB. Flags duplicate → rejects submission. User B leaves a comment: “this was f***ing awful”. Profanity detected. QC blocks submission, prompts user to revise. User C rates an item with a clean submission. QC passes it forward to aggregation
---


## Aggregation Module


### Aggregation Strategy Overview
Our aggregation strategy is designed to produce a clear, stable rating for each dining hall menu item based solely on clean, valid, non-duplicate ratings that pass the QC module. Because eating experiences are subjective and every Penn student is a valid evaluator of dining hall food, we intentionally avoid complex weighting strategies. Once QC removes inappropriate or repeated submissions, every remaining rating is treated equally.
This produces a system that is intuitive for users—each rating counts exactly once—and easy to explain. Aggregation focuses on computing the average rating, the number of contributors, and the variability of the responses. These metrics help end users understand not only what the current consensus is, but also how reliable that consensus is (e.g., items with only two ratings are labeled as “low confidence”). Overall, this approach fits the nature of Peli because it keeps the system transparent, aligns with user expectations, and maintains accuracy without unnecessary algorithmic complexity.
### Aggregation Method


**Primary method**: 
Mean rating of all QC-approved submissions
Rating distribution (counts of 1/2/3/4/5)
Confidence score based on number of ratings + variance


**Implementation details**:
- Input format: A list of QC approved ratings
- Processing: Group ratings by item and then calculate mean, standard deviation, variance
- Output format: 
{
  "item_id": "Hill_Pizza_2025-11-13",
  "aggregated_rating": 4.0,
  "num_ratings": 3,
  "confidence": 0.72,
  "distribution": {
      "1": 0,
      "2": 0,
      "3": 1,
      "4": 1,
      "5": 1
  }
}


- Handling edge cases: 
<3 ratings → mark as “Low Confidence” (not enough data)
Zero valid ratings → display “Not yet rated”
High disagreement (large variance) → lower confidence


**Why this method**:
[Justify why this aggregation approach is appropriate for your project and data type]
Food ratings are subjective → no "correct" answer → don’t weight users differently. QC already handles filtering → aggregation doesn’t need to detect bad data. Simple averaging is transparent and easy for users to trust. Confidence scoring adds nuance without complexity. High confidence = many agreeing users. Low confidence = few or extremely mixed ratings. This matches the goal of Peli: helping students decide whether it’s worth swiping into a dining hall based on the collective experience of peers.
### Aggregation Module Code Plan


**Location in repo**: [e.g., `src/aggregation/aggregate.py`]


**Key functions/classes**:
1. aggregate_item(item_id, cleaned_ratings): Computes average, distribution, variance, and confidence for a single item.
2. calculate_confidence(num_ratings, variance): Converts rating count + disagreement into a 0–1 confidence score
3. aggregate_all_items(cleaned_qc_outputs): Runs aggregation for every menu item in the daily dataset.


**Input data format**:
```
[
  { "item_id": "Hill_Pasta_2025-11-13", "rating_cleaned": 5 },
  { "item_id": "Hill_Pasta_2025-11-13", "rating_cleaned": 4 },
  { "item_id": "Hill_Pasta_2025-11-13", "rating_cleaned": 3 }
]


```


**Output data format**:
```
{
  "item_id": "Hill_Pasta_2025-11-13",
  "aggregated_rating": 4.0,
  "confidence": 0.66,
  "num_ratings": 3,
  "distribution": { "1":0, "2":0, "3":1, "4":1, "5":1 }
}


```


**Sample scenario**:
_Walk through a concrete example of your aggregation module in action_


[Example: "10 workers rate restaurant review sentiment. Scores: 7 positive, 2 negative, 1 neutral. Aggregation weights by worker reliability scores, outputs 0.82 positive sentiment with confidence interval."]


Three students rate “Hill Pasta” with scores of 5, 4, and 3. The QC module first checks each submission to ensure there are no duplicates and no inappropriate text, and all three ratings pass. The aggregation module then averages the cleaned ratings to produce a final score of 4.0, along with a distribution showing one rating in each of the three categories. Because the item has only a few ratings with some variation, the system assigns a medium confidence level. This aggregated result—4.0 with medium confidence—is then shown to users.
### Integration: QC ↔ Aggregation


**How do these modules interact?**
[Describe the relationship. Does QC happen before aggregation? After? Iteratively? Do they share data structures?]
QC always runs before aggregation. They share same data attribute of rating_cleaned.
**Data flow diagram** (if different from main flow diagram):
[Location or description]
User rating submission → QC module (duplicate + profanity checks) → QC-approved ratings table → Aggregation module (compute averages + confidence) → Aggregated results table → Frontend display


---


## User Interface & Mockups


### Interfaces Required


_You need mockups for ALL user-facing interfaces_


**For Crowd Workers:**
- [ ] Task interface / HIT design
- [ ] Instructions page
- [ ] Training/qualification interface (if applicable)


**For End Users:**
- [ ] Main interface
- [ ] Results display
- [ ] Any configuration/input screens


**For Administrators (your team):**
- [ ] Dashboard/monitoring
- [ ] Data management interface


### Mockup Details


**Mockup location**: [e.g., `docs/mockups/` folder or links to Figma/Marvel/etc.]


**For each interface, describe**:


#### Interface 1: [Name]
- **User type**: [Crowd worker / End user / Admin]
- **Purpose**: [What is this interface for?]
- **Key elements**: [What must be visible/interactive?]
- **Mockup file**: [filename or link]
- **Notes**: [Any important design decisions or requirements]


#### Interface 2: [Name]
- **User type**: [Crowd worker / End user / Admin]
- **Purpose**: [What is this interface for?]
- **Key elements**: [What must be visible/interactive?]
- **Mockup file**: [filename or link]
- **Notes**: [Any important design decisions or requirements]


_Continue for all interfaces..._


### Task Design (for crowd workers)


**If using MTurk or similar platform:**


**HIT title**: [Your HIT title]


**HIT description**: [What workers will see in the HIT listing]


**Task instructions**:
_Write the actual instructions workers will see. Be specific and clear._


[Your instructions here]


**Example task**:
[Show workers exactly what one complete task looks like]


**Estimated time per task**: [X minutes]


**Payment per task**: $[amount]


**Number of tasks per HIT**: [number]


**Qualifications required**: [e.g., >95% approval rate, >100 HITs, US location]


---


## Technical Stack


### Technologies


**Frontend**: Next.js w/ Tailwind CSS, 


**Backend**: Next.js


**Database**: PostgreSQL on AWS RDS


**Crowdsourcing Platform**: Custom web app and Penn students recruited via social media and posters


**ML/AI Tools** (if applicable): None for now. Possible lightweight sentiment analysis for the future using scikit-learn


**Hosting/Deployment**: Vercel, AWS RDS, AWS Lambda for daily scraping cron job


**Other tools**: Figma, GitHub, AWS CloudWatch for basic monitoring


### Repository Structure


**Current structure**:
```
your-repo/
├── README.md
├── docs/
│   ├── flow-diagram.pdf
│   ├── mockups/
│   └── ...
├── src/
│   ├── qc/
│   ├── aggregation/
│   └── ...
├── data/
│   ├── raw/
│   ├── sample-qc-input/
│   ├── sample-qc-output/
│   ├── sample-agg-input/
│   └── sample-agg-output/
└── ...
```


**Explain any deviations**: [If your structure differs, explain why]


---


## Data Management


### Input Data


**Source**: Daily menu data scraped automatically from the Penn Dining website via our scheduled scraper.
User-generated rating submissions collected through the Peli web application.


**Format**: Menu data - JSON objects representing each dining hall item. Rating submissions - JSON payloads sent from the frontend to QC. Database storage - PostgreSQL tables (items, ratings, users, qc_results)


**Sample data location**: `data/raw/`


**Sample data description**:
For the dining hall items scraped on a given day, the data might include: Item name, dining hall name, meal period (breakfast/lunch/dinner), date, item_id (auto-generated)


**How much data do you need?**
- For testing/development: ~10–20 menu items with ~10 sample ratings (manually created or simulated)
- For your final demo/analysis: At least 50 ratings from 10+ students, representing multiple dining halls and meal periods


**Data collection plan**:
Menu data collected daily via scheduled scraping (CRON job or Vercel Cron). Human-generated rating data collected after deployment through crowd recruitment (posters, class announcements, word of mouth). QC and aggregation outputs automatically generated as users submit ratings.




### QC Module Data


**Input location**: `data/sample-qc-input/`


**Input format**:
```
{
  "user_id": "andrew",
  "item_id": "Hill_FrenchToast_2025-11-13",
  "rating": 4,
  "comment": "pretty good",
  "timestamp": "2025-11-13T09:15:30Z"
}


```


**Output location**: `data/sample-qc-output/`


**Output format**:
```
{
  "qc_passed": true,
  "user_id": "andrew",
  "item_id": "Hill_FrenchToast_2025-11-13",
  "rating_cleaned": 4,
  "comment_cleaned": "pretty good",
  "meal_window": "breakfast"
}


```


**Sample scenario documentation**:
We will include in the README.md:
Example of a duplicate rating being rejected. Example of a profanity-containing comment being blocked. Example of a valid rating passing QC. Explanation of QC steps (duplicate check → profanity filter → validation → output)




### Aggregation Module Data


**Input location**: `data/sample-agg-input/`


**Input format**:
```
[
  { "item_id": "Hill_Pasta_2025-11-13", "rating_cleaned": 5 },
  { "item_id": "Hill_Pasta_2025-11-13", "rating_cleaned": 4 },
  { "item_id": "Hill_Pasta_2025-11-13", "rating_cleaned": 3 }
]


```


**Output location**: `data/sample-agg-output/`


**Output format**:
```
{
  "item_id": "Hill_Pasta_2025-11-13",
  "aggregated_rating": 4.0,
  "num_ratings": 3,
  "confidence": 0.66,
  "distribution": {
    "1": 0,
    "2": 0,
    "3": 1,
    "4": 1,
    "5": 1
  }
}


```


**Sample scenario documentation**:
In our README.md file we will include: Description of how ratings are grouped by item_id. Explanation of mean, variance, and confidence calculation. Example of low-confidence rating (only 1–2 inputs)




### Data Dependencies


**Does your QC module output feed into your aggregation module?**
Yes, Only QC-approved ratings are passed to aggregation. If QC rejects a submission, it is never included in aggregated results.


**Data flow between modules**:
Data flow between modules:


1. Scraper imports menu → stores items in database
2. User submits rating → raw JSON passed to QC module
3. QC module validates and cleans data
If QC fails → rating is rejected and stored only in QC logs
If QC passes → cleaned rating stored in ratings_cleaned table


4. Aggregation module fetches all QC-approved ratings → computes mean, distribution, confidence
5. Frontend displays aggregated results to users


Data always moves in one direction:
Raw rating → QC → Cleaned rating → Aggregation → Display


---


## Crowd Recruitment & Management


### Recruitment Strategy


**Where will workers come from?**
Penn community (those with dining plan), so mostly underclassmen


**How will you reach them?**
Visiting lectures (classes for mostly underclassmen) and advertising our platform
Making posters and put them throughout dining halls


**When will you recruit?**
Once the application is deployed to public


### Worker Incentives


**Compensation model**:
- Payment per task: $0
- Estimated time per task: 30 seconds
- Effective hourly rate: $0


**Or alternative incentive**: intrinsic motivation and gamification


**Justification**: students would love to share how they feel about certain menu items, and leaderboard for those who rated most food items with images




### Scale Requirements


**For MVP/Demo**:
- Minimum workers needed: 50
- Minimum tasks completed: 1 per user
- Timeline: Late November


**For Full Analysis**:
- Target workers: 300
- Target tasks: 1 per user
- Timeline: Mid December


### Backup Plan


**If recruitment fails or is insufficient**:
- [ ] Use MTurk/paid workers (budget: $[amount])
- [x] Simplify task to require fewer workers
- [ ] Use simulated/synthetic data
- [ ] Other: [specify]


---


## Project Milestones & Timeline


### Week-by-Week Plan


**Week 1 (Dates: [11/X17 -11/23])**
- Milestone: Think of main features/setup
- Tasks:
 - [ ] Come up with pages - James Doh
 - [ ] Basic infra setup through AWS - Joshua Lee
 - [ ] Check when dining hall menus posted every day - Katherine Yue
- Deliverable: Infra setup


**Week 2 (Dates: [11/24 - 11/30])**
- Milestone: Dining hall menu integration
- Tasks:
 - [ ] CRON job that scrapes dining hall data every morning -> writes to database - Eric Lee
 - [ ] Basic app set up through Next.js - Andrew Park
 - [ ] Route for fetching dining hall menu items - James Doh
- Deliverable: Push CRON job code/routes to repo


**Week 3 (Dates: [12/1- 12/7])**
- Milestone: Implement main functionalities
- Tasks:
 - [ ] Backend API endpoints - Joshua Lee, Eric Lee
 - [ ] Frontend - James Doh, Andrew Park, Katherine Yue
- Deliverable: frontend and backend code done and pushed to repo


**Week 4 (Dates: [12/8 - 12/15])**
- Milestone: Deployment + Gather data
- Tasks:
 - [ ] Deployment - James Doh
 - [ ] Creating posters - Joshua Lee, Eric Lee
 - [ ] Data analysis - Katherine Yue, Andrew Park
- Deliverable: final project + data analysis completed


_Continue through your full timeline..._


### Critical Path


**Blocking dependencies** (what MUST be done before other work can proceed):
1. CRON Job/dining hall menu fetching must be done before full stack development
2. Creating posters/advertisement must be done before data analytics


**Parallel work** (what can be done simultaneously):
- James, Katherine, and Andrew can work on frontend while Josh and Eric work on backend


**Integration points** (when pieces must come together):
- 11/30: Dining hall data with backend/frontend
- 12/15: Backend/frontend together and make sure they work well


---


## Risk Management


### Technical Risks


**Risk 1**: CRON Job/dining hall data fetching
- **Likelihood**: Medium
- **Impact**: High
- **Mitigation**: Comprehensive webscraping when menu for the day is available
- **Backup plan**: Likely not to fail


**Risk 2**: Complex deployment
- **Likelihood**: Low
- **Impact**: High
- **Mitigation**: Use Next.js/Vercel
- **Backup plan**: Will not fail since we have experience


### Crowd-Related Risks


**Risk 1**: Can’t get enough workers
- **Likelihood**: Medium
- **Impact**: High
- **Mitigation**: Focus a lot on advertisements, like posters
- **Backup plan**: Post on social media


**Risk 2**: Inaccurate data
- **Likelihood**: Medium
- **Impact**: Medium
- **Mitigation**: Leave a disclaimer emphasizing the importance of accurate data
- **Backup plan**: Use historical data for previously appeared food in combination so that inaccurate data averages out


### Resource Risks


**Risk 1**: AWS Budget Overuse
- **Likelihood**: Low
- **Impact**: Medium
- **Mitigation**: Use freetier and set up cloudwatch
- **Backup plan**: Use personal budgets


---


## Evaluation Plan


### What You'll Measure


**Primary metrics**:
1. Avg. Number of Ratings per Menu Item:
* How measured: Count total ratings per item across all dining halls and compute the mean.
* Target value: >= 5 ratings per frequently-served item (ex: pasta, pizza, chicken dishes, etc.)
2. User Engagement:
* How measured: DAU (daily active users) + WAU (weekly active users) from app logs.
* Target value: >= 30 WAU by demo week
3. Rating Reliability:
* How measured: Variance confidence score for aggregated ratings.
* < 1.5 variance for >= 60% of items


**Secondary metrics**:
1. Speed of Contribution:
* How measured: Median time between menu release and first rating on the menu.
2. Repeat Usage:
* How measured: Number of students who submit 2+ ratings in a week.


### Analysis Approach


**What questions will your analysis answer?**
1. Does crowdsourced data produce stable and reliable food ratings across days?
2. How quickly do students engage with menu items after they appear?
3. Does the confidence score meaningfully correlate with variance and rating count?


**What comparisons will you make?**
- [ ] Compare crowd vs. expert performance
- [X] Compare crowd vs. automated baseline
- [X] Compare different QC methods
- [ ] Compare different aggregation methods
- [ ] Analyze cost/quality tradeoffs
- [X] Other: Compare with/without QC filtering to quantify improvement


**Data you'll collect for analysis**:
- Timestamped rating submissions: Needed to analyze engagement patterns and QC effectiveness.
- QC-passed vs. QC-rejected entries: Needed to understand noise level and malicious/trolling behavior.
- Aggregated ratings and variance for each menu item: Needed for reliability and confidence-score analysis.


**Analysis methods**:
* Descriptive statistics: mean, variance, histograms of rating distributions
* Time-series analysis: rating activity over time each day
* Correlation analysis: number of ratings <-> variance <-> confidence score
* Visualizations:
	* Heatmaps of dining halls x average ratings
	* Distribution plots of 1-5 ratings for popular items
	* Confidence vs. number of ratings scatterplots
---


## Ethical Considerations


### Worker Treatment


**Fair compensation**: Although students are not financially compensated (intrinsic motivation-based system), tasks are short and provide direct utility: helping students make informed dining decisions. The platform avoids exploitative MTurk-like conditions since participants are voluntary users who benefit directly from the system.


**Informed consent**: Before submitting a rating, users see a brief notice explaining:
* their data will be aggregated,
* individual ratings will not be publicly identifiable,
* and submissions may be used for academic project analysis.


**Rejection policy**: Submissions are only rejected when:
* the user submits duplicates,
* entries contain profanity or harassment, or
* ratings violate the meal-window validity rule.


### Data Ethics


**Privacy**:
* User identities (PennKey) are never shown publicly
* Only aggregated ratings are visible.
* Internal logs store only the minimum required identifiers for QC (hashed user ID + timestamp).
* No geolocation, device metadata, or behavioral tracking is recorded.


**Consent**: Submission of ratings implies consent to use the data for:
* aggregation,
* system performance analysis,
* and class project evaluation.
A clear consent statement appears at first login.
**Data storage**:
* Data stored in a secure database (AWS RDS or similar).
* Access restricted to the development team.
* No third-party sharing.
* Data deleted after course completion unless explicitly anonymized for future academic use.


### Potential Harms


**Could your project be misused?**:
* If mismanaged, ratings could be used to unfairly criticize dining staff.
* Aggregated data might be misinterpreted as factual nutritional or safety assessments.


**Could it cause harm?**:
* Exposure of identifiable user data could compromise student privacy.
* Biased or low-sample-size ratings might mislead students.


**Mitigation**:
* Strict anonymity: never display user-level data.
* Label low-rating-count items with “Low Confidence” to avoid misinterpretation.
* Provide disclaimers that the platform reflects subjective user opinions, not official evaluations.
* QC filters remove harassing or defamatory comments to protect dining staff and community tone.


---


## Documentation Standards


### Code Documentation


**Each module must include**:
- Docstrings for all functions/classes
- README in module directory
- Example usage
- Input/output format specifications


**Current documentation status**:
- [ ] QC module: [Fully documented / Partially documented / Not yet documented]
- [ ] Aggregation module: [Fully documented / Partially documented / Not yet documented]
- [ ] Other modules: [List status]


### Repository README


**Your main README.md must include**:
- [ ] Project overview and goals
- [ ] Setup instructions
- [ ] How to run the system
- [ ] Where to find QC and aggregation code
- [ ] Data format specifications
- [ ] Team member contacts
- [ ] License information


### Ongoing Documentation


**How will you keep documentation current?**
[Describe your process for maintaining docs as code evolves]
With each team meeting (likely weekly), we’ll assign documentation tasks to ensure sections are updated as we go. On a smaller scale, individual PRs will have some description for logging and to make documentation smoother.


---


## Questions for Teaching Staff


### Technical Questions


1. Best way to handle items that appear over multiple days (may differ over time with same name)?
2. What if there’s no access to the Penn Dining API? Scraping?


### Scope Questions


1. Is having the mobile version a reasonable stretch goal?
2. How much of the project should be fully flushed out (vs. proof of concept with mock data)?


### Resource Questions


1. Any other paid services we may need?


### Other Concerns


[Any other questions, concerns, or areas where you need guidance]
None for now, thank you.


---


## Commitment


**We commit to**:
- [X] Building a working prototype with functional QC and aggregation modules
- [X] Creating comprehensive documentation in our GitHub repository
- [X] Recruiting and managing a real crowd (or simulated crowd)
- [X] Collecting sufficient data for meaningful analysis
- [X] Meeting project milestones on schedule
- [X] Communicating proactively if we encounter blockers
- [X] Treating crowd workers ethically and fairly


**Team signatures**:


- _________________________ James Doh, 11/13/2025
- _________________________ Eric Lee, 11/13/2025
- _________________________ Joshua Lee, 11/13/2025
- _________________________ Andrew Park, 11/13/2025
- _________________________ Katherine Yue, 11/13/2025


---


## Submission Checklist


This submission **is a working document**. You may not have finalized all version (of the flow diagram, the sample data, etc.), which is **acceptable**.


Before submitting this proposal, verify you have:


- [X] Completed all sections of this template
- [X] Provided team availability for TA meetings
- [X] Listed team skills and learning needs
- [X] Included point values for all components (total 15-20)
- [X] Described detailed implementation timeline
- [X] Identified risks and mitigation strategies
- [X] Had all team members review and sign


Then:


- [X] Set up GitHub repository with required directory structure
- [X] Prepared questions for teaching staff
- [X] Created flow diagram showing QC and aggregation modules
- [X] Created mockups for all user-facing interfaces
- [X] Added sample input/output data for QC module
- [X] Added sample input/output data for aggregation module


**Submission method**:
- **You are able to make multiple successive submission to iterate, complete this proposal.**
- Pull request to `ideation-fall-2025` repository, in `round5_final` folder
- Should be in the root of your GitHub organization


**Submission deadline**: Thursday, Nov. 13 at 11:59PM ET

