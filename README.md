# TigerApps Course Update Guide

# TL;DR

Note that it make take some time for each app to reflect new courses on its frontend due to the way things are cached.

**Once per semester:**

TigerPath updates automatically. Just manually verify that it contains the latest courses and/or reflect the new term name (e.g. Spring '25).

TigerSnatch updates course data automatically, but new notification intervals may need to be appended to its spreadsheet. See the [relevant section](#add-new-notifications-intervals-if-necessary) below for steps. Also manually verify that the app contains the latest courses.

PrincetonCourses updates course data automatically, but the course evaluations script must be run manually for the new term. See the [relevant section](#update-course-evaluations) below for steps. Also manually verify that the app contains the latest courses.

See the [contacts section](#contacts) if you need help.

# TigerSnatch

## Update course data

1. TigerSnatch will automatically update to the new term within 15 minutes of new courses being released.
   - **Verify** that TigerSnatch has updated to the new term by going to https://tigersnatch.com/dashboard and checking that the new term is shown in the status indicator (upper left), and checking that searching for a course works.

## Add new notifications intervals (if necessary)

1. Go to [this Google Sheet](https://docs.google.com/spreadsheets/d/1iSWihUcWa0yX8MsS_FKC-DuGH75AukdiuAigbSkPm8k/), which controls when TigerSnatch sends notifications for open spots. The sheet is also accessible from the TigerSnatch Admin panel.
   - You must be given edit access. To gain access, email nicholaspad@princeton.edu.
2. **Carefully** read the instructions in the sheet titled "README".
3. Switch to the sheet titled "Data" and enter new datetime intervals (if already added, verify that they are correct) for each course enrollment period and the add/drop period for the next semester.
   - Make sure to add a comment in the far-right column for each datetime interval entry.
4. Wait 10 minutes. TigerSnatch will poll this sheet automatically during that time.
   - **Verify** that the new datetime intervals have registered by hovering over the "status" badge in the upper left of the TigerSnatch website. The last sentence should begin with "Next notifications period:".

# TigerJunction

## Update course data

Follow the instructions in the [TigerJunction Update Guide](https://github.com/TigerAppsOrg/tiger-junction/blob/main/UPDATE_GUIDE.md).

# PrincetonCourses

## Update course data

1. PrincetonCourses will automatically update to the new term within 24 hours of new courses being released.
   - **Verify** that PrincetonCourses has updated to the new term by going to https://princetoncourses.com and checking that the new term has been added to the "Semester" dropdown, and checking that searching for a course works.

## Update course evaluations

### One-time setup

1. Create a file named `.env` in the PrincetonCourses source code root directory and enter the following key-values pair (with quotes but without angled brackets):
   - `MONGODB_URI='<PrincetonCourses production MongoDB connection string>'`
   - Obtain this URI from the Config Vars section in the PrincetonCourses production web panel in Heroku (go to the Settings tab).
2. Define the following environment variables through the Terminal (e.g. using `$ conda env config vars set <key>=<val>`):
   - `CONSUMER_KEY=<MobileApp API consumer key>`
   - `CONSUMER_SECRET=<MobileApp API consumer secret>`
   - Obtain these values from the production Config Vars section as well.
3. Install the Python `requests` library (e.g. via `pip`).

### Perform update

1. Execute the below command in the PrincetonCourses source code root directory.

```
$ node importers/scrapeEvaluations.js
```

2. Follow the instructions printed by the script. Specifically, you'll need to retrieve a cookie value that allows for CAS authentication. After this, when prompted for a MongoDB query, enter: `{"semester": xxxx}` where `xxxx` is the term code you want to get reviews from (e.g. 1224), typically 1-2 semesters ago.
3. Let the script complete (takes about 30 minutes).
   - **Verify** that new evaluations were scraped by searching for a course (e.g. COS126) on PrincetonCourses and checking that evaluations from the previous term were added.

# Contacts
Contact the following if you need help or if you have questions:

- **General** - it.admin@tigerapps.org
- **Joshua Lau** - motoaki@princeton.edu
- **Leo Stepanewk** - leo.stepanewk@princeton.edu