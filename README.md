# TigerApps Course Update Guide

# TL;DR

Once per semester:

ReCal and TigerPath update automatically. Just manually verify that the apps contain the latest courses and/or reflect the new term name (e.g. Spring '23).

TigerSnatch updates course data automatically, but new notification intervals may need to be appended to its spreadsheet. See the [relevant section](#add-new-notifications-intervals-if-necessary) below for steps. Also manually verify that the app contains the latest courses.

PrincetonCourses updates course data automatically, but the course evaluations script must be run manually for the new term. See the [relevant section](#update-course-evaluations) below for steps. Also manually verify that the app contains the latest courses.

See the [contacts section](#contacts) if you need help.

# TigerSnatch

## Update course data

1. TigerSnatch will automatically update to the new term within 15 minutes of new courses being released.
   - **Verify** that TigerSnatch has updated to the new term by going to https://tigersnatch.com/dashboard and checking that the new term is shown in the status indicator (upper left), and checking that searching for a course works.

## Add new notifications intervals (if necessary)

1. Click the yellow "G-Sheet" button in the TigerSnatch Admin table. This Google Sheet controls when TigerSnatch sends notifications for open spots.
   - You must be given access to this Google Sheet. To gain access, email nicholaspad@princeton.edu.
2. **Carefully** read the instructions in the sheet titled "README".
3. Switch to the sheet titled "Data" and enter new datetime intervals (if already added, verify that they are correct) for each course enrollment period and the add/drop period for the next semester.
   - Make sure to add a comment in the far-right column for each datetime interval entry.
4. Wait 10 minutes. TigerSnatch will poll this sheet automatically during that time.
   - **Verify** that the new datetime intervals have registered by hovering over the "status" badge in the upper left of the TigerSnatch website. The last sentence should begin with "Next notifications period:".

Contact nicholaspad@princeton.edu and shannon.heh@princeton.edu if you need help or if you have questions about any of these steps.

# ReCal

## Update course data

1. ReCal will automatically update to the new term within 24 hours of new courses being released.
   - **Verify** that ReCal has updated to the new term by going to https://recal.io and checking that the new term has been added to the tab bar, the old one has been removed, and that searching for a course works.

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

2. Follow the instructions printed by the script. Specifically, you'll need to retrieve a cookie value that allows for CAS authentication. After this, when prompted for a MongoDB query, enter: `{"semester": xxxx}` where `xxxx` is the new term code (e.g. 1232).
3. Let the script complete (takes about 30 minutes).
   - **Verify** that new evaluations were scraped by searching for a course (e.g. COS126) on PrincetonCourses and checking that evaluations from the previous term were added.

# Contacts

- **Nick Padmanabhan** - nicholaspad@princeton.edu
- **Shannon Heh** - shannon.heh@princeton.edu
- **Adam Gamba** - agamba@princeton.edu
- **Ben Chan** - bychan@princeton.edu
